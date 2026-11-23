# English Narration Script

Now let's move on to the next chapter, Chapter 8.

In Chapter 7, through introducing the boot of our system and printing a basic HelloRV OS on top of it, we learned how to get a RISC-V board up and running, on top of it, and then used its most basic peripheral, the serial port device.

Next, let's turn to Chapter 8, which mainly introduces, building on the previous foundation, how we can further manage the memory of our system, taking that management a step further. This part of the content is mainly divided into two parts.

One part is an introduction to how to implement dynamic memory allocation, and then, based on this first part, in terms of implementing the concepts we introduced, we can implement, at the page level, at the configuration level,

some calling interfaces for memory allocation and management. I'll mainly cover this in two steps. As for the relevant reference materials, I've listed some of them here as well. In our slides, they'll actually be listed too.

When we get to some important concepts, I'll show some of the figures and tables, and where they come from in the references. You can go back and take a look. So let's start with the first part. That is, further management of memory.

So here, before we continue further development, we need to take the memory and manage it further. Why is that? It's because from what we saw earlier, What we've been learning is that when it comes to memory management, up to this point,

we've actually already encountered two kinds of memory management approaches, right? The first one is called automatic memory management. Now, this way of managing memory, we actually implement through the stack, through this mechanism.

This actually corresponds in our C language, to what is essentially used, namely the automatic variables defined inside functions. Now, we know that these automatic variables, this kind of memory, when you enter a function, it gets allocated inside the function stack,

and when the function exits, it is actually released, right? Once it goes out of scope, it's released. You can think of it as a kind of automatic management. Now, there's another kind. What we're studying is static memory. In our code, this corresponds to,

basically, defining some global variables, or static variables. Now, for these variables, we're actually using them in a static way. We know where this static memory is placed. It's actually placed after we compile, in some special data segments.

Right? In the data sections. Then they get loaded into memory, placed in these data segments. Okay. So, for these things, what we can see is that, beyond this way of using memory, Actually, there's another kind of demand for memory

which is that we might need to at runtime dynamically allocate some memory. Because in real-world programming, we know there are some data structures whose memory size actually, at the point when the program starts running, cannot simply be placed

entirely into static memory, or stored on the stack. So in C, for example, what we have inside is, say, we use malloc, or free, that kind of dynamic approach. Now, this dynamic approach brings in the concept of a heap. And this heap,

under the hood, exactly where So, for example, when we normally call malloc and free, behind that, exactly where is that heap located in memory? Well, in fact, in our at the very lowest level, it's actually maintained for you by our operating system.

So how do we build that heap? Today, we'll mainly focus on that topic. Alright, let's first look at this table. We've seen this table several times before, the memory map. From what we learned earlier, we know that on this table,

the Bootloader ROM region We actually can't touch it. Because you can think of it as a block of read-only memory that's baked in there. What we actually work with more is the DRAM. That is, this block up here. This block up here.

Now for this block up here, we know that after going through the mapping, it gets mapped to our physical memory, from 0x8000 up to 0x88000000, over this range of addresses. Now for the memory in this address range, actually, how it's used internally,

we haven't done any deeper management of it. Think about it, right? Because as you know, let's look at this diagram down here. Actually, for this whole area on this side, like on this Virtual board we're currently using, for this memory in this 128-megabyte range,

we haven't actually done any further or deeper management. Why do I say that? Well, as you know, after we compile a program, and those of you who did the earlier ones, you know that what we compile here is actually an OS.ELF,

an executable file like that. Now, this file, as we know, based on what we covered earlier, is actually in ELF format, right? And in this ELF format, there are corresponding sections. Then during loading, it is loaded segment by segment.

When this data, is loaded into memory, it will actually only take up a portion of our 128 megabytes, part of the space, and the rest There is actually a large chunk of space This chunk of space we actually have not made use of it

So later on, for the dynamic allocation we want to do, the idea is basically to find a way to use this memory at the back, which is what we call the heap, this memory heap, and put it to use. And by using a dynamic approach, we can, in our programs,

allocate when we need it, and return it when we no longer need it. For the sections below, as we know, once they are loaded into memory, their size cannot be changed. So if we want to make use of this part, then the next question arises

So if we want to make use of this region, what do we need to do first? As you all know, first you have to know where this region actually starts, and how much space is actually left. These are all things we need to know, because only after knowing this information

can we take this chunk of memory and manage it in a finer-grained way. Now, some of you might say, can't we just calculate it ourselves? Well, of course you can. Because in our... But of course, that's really cumbersome, because basically you'd have to know

the sizes of all the variables that were allocated in there, and then compute it yourself based on that. But in fact, there's really no need to do that. Because as everyone knows, once you have a compiler and a linker, during the compilation and linking process,

the compiler and linker can actually figure out these sizes for you automatically. We don't need to calculate them by hand. So what we'll do next is look at how to get the size of this region and its starting address. At the same time, we actually make full use of

some features of the compiler and linker to help us do this. So let's now briefly review the compilation and linking process, as well as some related concepts. Let's see how to get this done. That is to say, this diagram should be quite familiar to all of you.

Basically, we no matter how complex your project is, actually the whole compilation and linking process It's all pretty much like this a diagram like this one That is to say, you might have multiple source files, and each source file,

you turn it into one they each become a .o file. And then in the end, these multiple dot-o files get linked together, and become one executable file, right? This is the basic process. Now, in this process, as we know, if we show it with a diagram,

we know that every dot-o file is essentially something like this, because we said each dot-o file is in the ELF file format. So this diagram, As you can see, compared with the complete ELF format we had earlier, what's the difference?

In this one, there is no program head table, right? What does it have? It only has the section table. This, mainly because of what we covered earlier, that is, a .o file, since it's not the final executable program, that program table,

it's used for loading. So only when it's an executable program, will there be that program head table, right? Just a brief mention of that. With multiple .o files, naturally there will be multiple of these the file of that ELF Now, in the end,

we want to turn it into an executable file, and inside this executable file, there are these program headers. See? Am I right? So now here is a question: when we actually need to construct such a final executable file like this,

what exactly do we need to do? Think about it — as a linker, what do you have to do? You actually need to take each one of these, which in fact has its .text section, and has some .data sections, and .bss sections, right? Then it needs to take what is in here and

Gather up all of the .text sections and merge them together. In fact, in the final executable file we produce, there is only one .text section. So in reality, the linker takes it and merges it all together, merging it all together.

And accordingly, for the other sections, there are similar operations as well. So what we actually need to do during the linking process is the following. That is, we need to take the sections with the same name that are scattered across different .o files,

and bring them all together. At the same time, we also need to during this linking process Actually, we also need to tell our linker where these sections should be placed. Think about it — isn't that right? Because ultimately we know

this thing has to be loaded through the program header table, and we need to load this thing into memory to run it. So then, this loading — loading it to where? Loading it to what location? All of these details actually need to be reflected

in this file. So how does the linker know all of this information? Well, our linker knows it through something called a linker script. to describe this information, all of the files up here are what we call input files, input files.

And the final executable file we generate, we call that output files. These related concepts are all covered in the GNU linker documentation. The reason I bring them up here, is to make it easier for you to go read the terminology in the relevant documentation later.

So describing these input files— what sections they have, where in the output file they should be placed, and when they are loaded, what information they get loaded to in memory— all of this information is described through something called a link script,

a so-called linker script, which is how it's described to you. As for this linker script, you can actually write it yourself. When we normally type GCC, why don't we write this thing? It's because GCC actually provides a default linker script.

If any of you are interested, you can actually, that is to say, when you normally type GCC, if you add an argument after GCC, for example, something called Verbos, with this Verbos argument, when you hit enter, let me see, this should be the linker script, right?

Normally, if you type L, that is, we know that this, this GCC, when we type GCC, actually, in the final linking step, it will actually go and call a... is implemented by a program called ld. Now, with this ld program, look — if at this ld step,

if I print out, adding a verbose flag, that is, printing out all the detailed information, you can see that at this ld stage, it prints out some very interesting information. This information, in fact, comes from its default linker script —

it is the information pulled out of it. That is to say, we actually don't have to write this linker script ourselves. But, for us, since we are now writing a kernel, this kernel, it's actually not like when we write applications.

Just now I demonstrated to everyone, that is, when we compile an application program, there is a default linker script. We can just use it directly. But since we are now writing a kernel, then this kernel's the final file of this kernel,

it is also an ELF file. How should this ELF file be organized? Where in memory should it be placed? This is something that we generally need to write ourselves at the low level. There is no default one. It varies depending on different boards,

it varies depending on different hardware. This is the concept of the linker script. So later on, we will actually use the linker script to obtain our Like we mentioned earlier, take the heap, for example, the memory location and size information of where it lives,

we don't have to As long as it's all clearly described in here, we don't have to in our code compute any of that ourselves. So how is it actually done? We'll look at that later. So for this linker script, when we compile, how do we tell the linker about it?

Say we've written this script, this script we wrote is a text file, how do we tell the linker about it? Well, we can actually use a -T argument, that is, GCC's -T argument, which we actually touched on earlier, so you can go back and review that.

So, earlier on, in those hello rbos exercises, if you actually take a look, including that earlier one, when we were writing assembly, in those test examples, they all had a -Ttext thing, right? Why is that value 0x8000? Now everyone should understand it, right?

So there is this -T parameter, but when we used this -T parameter back then, we used it in a very simple way. It really just simply specified, that is, for the executable file we were compiling, its starting position, we just told it a starting position of 800,

and actually nothing else was specifically specified for it. But now, since we need to build a kernel, we need to tell the linker more information it's not enough to just give it a starting address so we need to write all of that information into a special file to describe it

and once we have this file our original -T parameter will become a bit more complex what we actually need to change it to is a -T followed by that OSLD file this file is our the linker script file for the OS we've been building here

change it to this form and that's it so if any of you are interested you can go and take a look at our example that is, starting from the third step our starting from our example today in our Makefile Now the -T parameter changes

to point to an OSLD, a linker script file. This is what we want to show you now. So next, let me introduce to you this linker script file, roughly how to write it. Now, for the linker script, the detailed usage instructions for this file

you can refer to the reference 1 we listed for you earlier, in that GNU binutils manual, there is a whole chapter dedicated to this topic. Now, since the tools we use here are mainly GNU tools, I'll introduce GNU's LD, the syntax of its link script.

Now, this link script, it's just a simple plain text file. Actually, you can use whatever editor you like, and you'll be able to write this thing. But inside it, it has a certain, it defines its own special special syntax, a special syntax.

Now, its syntax is also fairly simple. We can think of it this way: a complete link script, this script, is actually made up of multiple commands. And each command has its own keyword, along with some of its parameters, just one command after another.

Then, apart from these commands, there are just some comments. And the comments, It supports the same thing as C using that pair of slashes to comment it out and just wrapping it up like that will work Now, when it comes to using it

as I mentioned just now you just follow GCC with that -T parameter to tell it the path of that linker script file and that's it So, here let me show you a simple example For instance, this SECTIONS you can think of this thing as just a single command

This is the simplest linker script In this linker script there is only one command and that command is the SECTIONS command Inside it these can be considered as some parameters of this command As for the specific meaning inside this

when we get to the examples later we'll introduce them to you then So this is the simplest lab script. Now, before we go through this lab script in detail, what we mainly want to tell you is this. Because if you go look at the Tiger documentation,

the commands in it are quite a few, there are a lot of them, but we don't use all of them. We only pick the ones we care about, that is, some of the ones used in this lab, and we pick a few to explain to you. Now where is this OS

lab file located? Right here in this spot. Starting from the second one, which is the memory management project, we brought in this OSLD, which is this file. So let's follow this example and go through some of the things inside it for you.

So this is the comment Its comment syntax is basically the same as C syntax We'll just just mention this briefly here Here I've listed different commands This is one command This is one command This is one command Now for each command

I've given some of its origins and a brief description You can also look at this Of course, in our class I will also go over them briefly for everyone Next, let's take a look at which commands are commonly used So the first one I want to introduce to you is

a commonly used linker script command called entry entry, as we know, in English means which means entry point Now, since it's the entry point it's actually through this command that we're setting, telling the linker the first instruction of our program to run

should be placed at which position in memory That's actually what it expresses Its syntax is very simple It's just ENTRY followed by a symbol symbol means a symbol It doesn't write a value directly That is, because we know as programmers

we actually can't determine right away at what position this value is So here we actually use a symbol to tell this linker Now let's take a look where this place of ours is Well, it's right here Now, as you can see, this is our

in here, it's written as start And as soon as we see start it should be quite familiar to everyone because we know that this symbol, start, where did we define it? Actually, it's defined in our first assembly file for instance, this one of ours,

the .S file in here, right? Back when we were talking about the first example, we already covered this that is to say, right here, this is actually where our program runs, where our kernel runs the first one specifies the location

we just take this identifier and fill it in here, and then the linker will know where our program starts running. Alright, this part is relatively simple. Now there's another one, called OUTPUT_ARCH, which is basically telling us

that the output file, the output file, as we know, is the os.erf that we generate, this ELF file, this output file, the computer architecture it is intended for, because it needs to know whether this final thing we generate is,

say, for x86, or ARM's, or this RISC-V, so what we need here, in our case, since this targets RISC-V, so at this place, we just fill in a string it defines itself, called Risk5, and that's it. Now, note one thing: it doesn't specify here whether it's Risk5,

32 or 64, right? So when we compile, how do we tell it whether we're generating 32 or 64? Actually, it's through a further specification in the GCC options. Let me mention this as well. That is, in our Makefile, when we compile at this point,

didn't we add some of these parameters? You can take a look. If any of you later want to ask about this, let's just explain it right here. Because I saw some of you asking in the group chat as well, basically why we're using this RISC-V GCC.

What did we fill in here? Let me take a look. Here we have it says 64 here. So why does it compile to 32-bit? Well, the 64 here only indicates that we our toolchain that we're running is running on a 64-bit host machine. This concept of cross-compilation,

we talked about it earlier, right? Our toolchain runs on a 64-bit machine, but the target program we generate is 32-bit. Don't get confused about this concept. So how do we tell a machine running 64-bit, If it wants to generate a 32-bit target,

it's actually conveyed through these parameters. What these 32s mean, I think everyone should be clear about by now, so I won't go over it again. You can look up the concept of ABI yourself, it's in the manuals, you'll find it.

Well, since it's mentioned here, let me just say it. Now, here, we only specify RISC-V, but we don't say 32 or 64. The specific 32 or 64 is specified through GCC's compiler options. Just want to point that out to everyone. Alright.

So that was the second point, specifying the generated architecture. Third, the memory command. This command is very important. It's actually telling Because, as you know, a linker has no idea about the actual target machine we run on

and what its memory really looks like. So how do we let the linker know? We need to tell it through the MEMORY command. Now, the syntax of MEMORY is very simple. First comes a name. Talking about this directly might be a bit dry,

so let's just look at an example. Let's look at the meaning of this example below. Each line of this thing can actually be thought of as defining one memory region. So the two lines below it, effectively define two pieces of memory:

one is ROM, and the other is RAM. You can tell just from the names. What it defines is, let's say, the first block of memory is is a read-only memory The second block of memory, is a read-write memory. RAM, that is. ROM. So, origin,

length, if you just know some English, you should be able to read this. Right? What is it actually telling us? It's saying that this block of memory, its starting address, origin is the starting address, is 0. And the length is 256K.

Of course, this address definitely refers to a physical address. Basically, in our course, we don't really want to introduce the notion of virtual addresses. Because that would drag in a lot more stuff. Here we only talk about physical addresses.

That keeps things simpler. And easier for everyone to pick up. So then, the first block of memory, you can think of it as, in our address space, at the very beginning, this is ROM, and its actual address is 0. And then, it is 256K in total.

And then, the second block is RAM, and its address is, let's say it starts from here. This range of memory. Its starting address is, simply, That's this thing. And the range is four megabytes. So it's really just describing this information,

that is, this way, we pass this information to our linker, and then our linker knows our memory layout. Then, at the final loading stage, the loading has to rely on this information to perform the loading. Now, this one, this thing here,

this RX thing, what is it actually telling us? Well, this is a bit complicated to explain. That is to say, its let me briefly describe it. If you feel it's not explained clearly, take a careful look at the manual, which has a detailed description of this parameter.

It actually, for example, let's take this as an example, take this as an example. It's actually saying that we know that all of our this linker script concept, it's nothing more than saying, take some input sections, and finally, put them

into an output section, right? So in here, aren't there a lot of sections? right? There's also this in there, the final target we want to generate. Now, later on we actually have a section command that is meant to describe which of these in here

go into which of those in there. We'll definitely need to talk about that. We haven't covered that command yet. Now there could be a situation where, you might not have described it. Say we, in this in this linker script, we didn't describe

for example, this chunk of memory I got this we might not have described it. put it into this one here which section does it go into? you didn't specify that Right. So if our linker finds that you didn't specify it, it will go by what's in here,

that is to say, if this section of yours is not specified by you, if it finds that this spot is a section with read-only attributes, then by default it will just put it into the ROM area to be loaded. That's actually what it means to express.

It's used to handle some default ranges. So under default conditions, in the default case, normally, we will override the information in here, so this may not have a particularly obvious effect. Some of them are mainly used to handle certain default cases.

I'll just briefly go over this. If any of you are interested, you can go and look at it yourselves later. The key thing is to understand this origin and length, and what information each line is expressing. That's the memory command.

Now correspondingly, our linker script, we also have a memory command. Where can you see it? Here it is. Right here. Now here, we actually only defined one RAM block, right? This RAM block, its address is 0x80000000. We've mentioned this value multiple times already.

Its range, is 128 megabytes. So on this virtual board of ours, this memory region has exactly this size. Now, the name here, we'll use it later on. Just to mention it here, when we talk about sections later, we will actually use this.

Keep that in mind as you read through it. It gets used right here. So what's used here, is basically telling us, for this text section, it will actually be placed into RAM, into this memory region. That's really all it does. Just to clarify that.

All right. So that's the memory command. And this one here, is the section command. This command is the one in our general linker script that is also the most important, and takes up the most space as a command. It is essentially

telling our linker how to take the sections in the input file, these sections, and map them into the sections of the output file, and how to place these sections in memory, that is, load them at what position in memory. It is really just specifying this information.

Now it says that besides being able to describe the output sections It can also be a symbol copy command, or other forms. We'll cover that in a moment. Let's take this example. Let me briefly walk through it. For instance, taking this example,

let's look at what this is actually saying. So this basically means that this section command begins here, and it wraps a bunch of braces around it. We just explained this part, which is that it essentially means that the contents in here

will ultimately be placed into rem, this block of memory. Now, as you can see here, there are many things starting with a dot. This thing, you can understand it as meaning It's describing because what it's actually describing is

in our final memory how things are laid out there how these sections are laid out in memory So you can think of it this way: during the layout process we always have a we have a current position current position current position,

or the concept of a counter a counter, or you could say a pointer and this pointer points to during the layout process, that current position So when this dot equals 0x100, that just means as I'm laying things out right now I'm going to start placing things at 0x1000.

So let's say down here is the low address, and up here is the high address. Then this pointer moves from the bottom upward. So say it reaches this position, and let's assume this position is 0x1000, a value like that. So what do we do starting from this position?

I'm going to start placing my text section. We're going to start placing our text section. So this text section, it goes right here, this dot, this place is the .txt text section. So here, it actually describes our section name in the output file,

for example, called .txt, and then followed by a colon. After the colon, there's a pattern. What is this? The content in here, you can actually think of it as describing, because as we know, all the sections in the output file come from

the sections in the input files, right? Now this * is a wildcard, meaning all the text sections from the input files get placed into the text section of the output file. And the location where it's placed, at load time, will be put at the memory location 0x001000.

Actually, it's describing this information. It's really describing this information. Okay. So after that's described, Now look at the next line, it's a dot again. In other words, this takes the pointer and moves it up again, moved to what position?

It's moved to 0x8000. Assume this position is 0x8000. Assume this position is 0x8000. Then, starting from this position, what do we place further up? We place the .data section and the .bss section. So this is where the data section is,

this is where the data section is. Then further up, what goes here is the .bss section. Now, where does the content of the data section come from? It's described right here: it's the Data sections of all the input sections. And the BSS section?

It's the BSS sections of all the input sections. Actually, through this description, we can actually see how, in memory, our program is laid out, and it is through this method that it gets described. This is what the linker script

achieves. Now, in our actual code, our content is a bit richer than that. As you can see, here we start defining our sections, and right here, quite a few are defined, including the text section, our rodata section, and so on and so forth.

It is through this single line to describe it, and so, as you can see, apart from the earlier things I just mentioned, we also see that in here statements like this are defined: provide, which is also a command. This command is really the

one we talked about here. Besides the autosign description, there can also be symbol assignment commands. For symbol assignment commands, we use a new command here, called the provide command. The meaning of this provide command

is very simple. It is actually equivalent to what we do in the C language to assign a variable for it And later, its syntax is simply an equals sign taking the content of this expression and assigning it to such a symbol a variable name.

So for example, right here we gave an example like this. So it assigns this thing to it. Now we know that this dot actually represents the concept of the current location. This location may be changing during the description of this section.

So this is definitely also a value, and this value is assigned to such a symbol. So this symbol, when we program later on, we can use it. That's because to the linker, it puts this thing into the final symbol table. And in the symbol table,

we can use it like a variable. Now as for our related code example, we can see it right here. Look, here actually, if we take this as an example, we can see that it's essentially saying, it's defining a variable name, defining a variable, defining a symbol.

This symbol, starts from this location, and then, here, it defines another one. So basically, as you can see, these two lines actually express something. We now know the start position of this BSS section and its end position. We take the start position

and put it into this symbol, and the end position goes into this symbol. So we've essentially obtained this information. Now, with this information, we can go further and compute the starting position of the entire memory, right?

Origin — this RAM — we know where the starting position of this RAM is. This value, what is the value of this expression? The value of this expression is actually We just defined the origin earlier in the memory section which is 0x8000

right? And at the same time we can also get at the same time we also get the heap start we know because our our heap memory is actually placed right after the BSS so once we have the BSS end after we get its end position we can take that end position

and actually use it as the starting position of the heap this way we get the correct address as for what this value actually is we don't really need to calculate it ourselves the linker, based on this linker script during the linking process

it will automatically compute this value for us and store it into these symbols So that achieves our goal Right? So these are just some introductions to using linker scripts Of course, right now the main focus is to show you how we use this linker script

to compute these memory layouts and ultimately, achieve our goal, which is to get the starting position and size of the heap So, let's take a look at how we can get this information in the code. Now, this diagram of ours, we should always keep it

in our minds. So now, through the linker script, So what did we actually do? First of all, as we know, based on what we just described, in the linker script, we defined the size of the memory, starting at 0x8000, with a length of 128 megabytes.

So through this, and then further down below, combined with these two provide commands below, we can get two symbols. One is the symbol start, and the other is the symbol end. This symbol, with these two symbols, we can actually obtain

the entire memory, our entire memory space. This seems to be written wrong. The diagram here is drawn wrong. It should be this, it should be this place Sorry about that. This should be this position. No, that's not it. We then get the entire

size of this. Right, we get this size, and the end position of the entire memory, the 70 position, the ending position. This is the first step. Then, at the same time, based on what we defined over here, these section positions we set up,

what else can we naturally get? We can get these text sections, data sections, and the starting and ending positions of these bss sections, right? We just introduced these to you earlier. So once we have this information, So naturally, we can use this approach

to further calculate the size of this heap its size, as well as its starting location. So in this way, through the linker script, we have obtained the specific locations of these regions that we care about. We don't have to compute them ourselves.

But just having this is not enough, because what we ultimately need is to be able to get these values in our code. So how do we get these values? It's actually quite simple. Once we are in the linker script, you can see this diagram here,

which is really just the information described in a linker script. So this diagram is actually just, you can think of it as the descriptive information of the linker script. So once we have this linker script information, we can actually

do one thing: in our code, and this code, is actually written in assembly, we can actually define these corresponding variables. We know that this .word is actually allocating a space, you can think of it as defining a variable,

and this variable is actually 32 bits, 4 bytes. For these bytes, we can actually take this symbol, we now know the linker knows the value of this symbol, and we take the value of this symbol and assign it to this variable, right?

So basically what we're doing is Now, this piece of code, where is it? Let me walk you through this code. It's in this new project of ours, in memory.s. It's in this file.

In this file, we're actually defining some variables, some global variables. You can think of these global variables as whose values are essentially obtained through this method, so we don't have to compute them ourselves. Now, the actual values,

they're actually calculated by the linker, during the linking process, computed for us. So once we have these global variables, we can then go ahead and use them further. We actually just you can think of it as we just said this is assembly code

so in our C language code, how do we use these variables? Actually, you can just think of them as global variables right? Just declare them as global 32-bit variables. Then in our C language code, we can use these variables directly.

As you can see, why are we able to use these variables to compute these values, getting the starting address of the heap? Actually, it comes from here, from these heap values above. So where do these things come from? They come from here.

So ultimately, where do these things come from? They're actually calculated from the linker script. Our entire logic here, is really just this process. Once you understand this, reading this code becomes pretty straightforward.

So this is using the linker to obtain all this relevant information. But this is only Actually, this only implements the first step of our dynamic memory allocation. This is the first step. Because right now we've only obtained

the size of this memory, that is, the size of this heap, and its starting position. So how do we actually implement it? Well, next So let me walk you through how to implement the related interface. Now, at the configurable granularity level,

let's take a look at how we implement so-called configurable-level memory allocation and deallocation. This, let me first explain to everyone, namely, when we normally call malloc and free, this kind of allocation, at what granularity level do we actually allocate?

We actually allocate at the granularity of our own units, right? That's a very fine-grained allocation. But if we look at it from the lower level, So in our operating system, allocation actually happens first at the page level.

And page allocation is usually done with a size of 4K, that is, 4K bytes as the allocation unit. Then, actually, malloc and free are built on top of this 4K page-level allocation, going one step further with what is essentially a second round of allocation,

a finer-grained allocation on top of it. Here, first let me walk you through page-level allocation. As for byte-level malloc, we actually leave that as an exercise for you to do on your own. So let's first look at the page level,

that is, coarse-grained allocation, and how it is done. The idea is It's actually pretty simple. Let's take a look at it together. So once we have this, now that we know the HIP, its size and starting position, in that situation,

we can actually take this HIP region, the area shown in blue, and build our own dynamic allocation management on top of it. In terms of how we implemented it, we wrapped it up and provided two interfaces. One is, PageElect, which is the allocation interface,

and the other one is free, which is the deallocation interface. Now for this allocation function, its parameter is an int value. Actually, what this means is we can since we are allocating in units of pages, right we essentially tell our

upper-level users, that is, when you call this interface, it's basically like saying you're telling us how many pages we should allocate, for example, whether you want one 4K or two 4Ks like that. Now, the return value, it's a pointer

that essentially points to the allocated region, which can be a single page or multiple pages. So please pay attention to this. this region its start address Then when it comes to freeing, that is, we take the one from just now the address that was returned,

and pass it back in, and then our kernel takes this region, essentially, so to speak, and recycles it. Then next time we can keep using it again. That is actually the effect we achieve, and that's all it takes. These are the two interfaces

we need to implement in the lab. Now, as for the actual implementation, there are actually many different approaches. We will briefly introduce one approach to you, and we'll explain everything using the simplest possible way.

So one approach is through a linked list right We've all studied this data structure haven't we So with the linked list, the ones in red represent the memory that has been allocated, and we just link the blocks together one by one.

Then, the ones in blue are essentially another linked list, which maintains the blocks that have not been allocated. So we can — this is one way of doing it, right? But actually, we can also use a bit-map based approach, a simpler approach,

which is that we can actually take the chunk of memory to be allocated We can think of it this way. Up front, we use some a region like this, which can actually be considered to be a piece of description information, that is, it describes what comes after it —

the real configuration is placed at this position. These are the individual 4K configurations. So this red part is describing which ones we have, through indexing, through indexing, that is to say, if, let's say, this is configuration 0,

then over here what it corresponds to is number 0, 1, 2, 3, 4, like this. They are managed through indexing. So in here, it's actually just used to describe whether a particular configuration has been allocated or has not been allocated,

and that's all you need. This way, you don't need you don't need to use pointers or linked lists. The advantage, is that it's relatively fast and quite simple to implement. What we actually do later on is to use an array-based approach

to implement this configuration for you — the allocation and release at the configuration level, we do it using an array-based approach. So this, now we know, actually, all of this information, we actually put it in our heap, in the heap.

So as for KeepStart, from our earlier analysis, it actually marks the start of this heap, right? To that end, we also need to define a because our real, the actual configuration, is actually placed here, starting from here, the memory actually given to users

actually starts from this area here. The part before it, you can think of it as memory we use for some internal management data. So this area, we actually don't expose it. for the end users to use. So we defined another new position here,

which is allocated_start, and this region corresponds to the memory we actually hand out. Then this block, this block is actually described by this thing here. And this block, in turn, is described through this position here. So this information,

the thing in front here, we can think of as individual items used internally for management, call them descriptors, for example, that works too. So we have this setup. Now this heap_start, we can compute it from this, which we already explained to you earlier.

And this allocated_start, So we actually compute it in this way. Now, this part, let me briefly go over the code here, that is, these 8 page sizes here means that we use the front part to store management information. We reserved about 8 page sizes.

One page size is 4K. So we used this large area here, to store this descriptive management information. And since we can roughly estimate it, because we know the whole memory is only 128 megabytes, so if we put this much information up front,

and based on each one of these, how much space one entry of management information roughly takes, we can actually roughly calculate that this space here is sufficient. So we reserved this much. Then, starting from this position,

plus this thing. Add that, and we get this position we get this position Now here, we at the same time, we also took this position and did an alignment operation on it Because on a computer, if it is aligned to 4K, the processing efficiency is higher

So we did a little bit of alignment first to make sure these addresses are all four-byte aligned So we got this position This is related to the code Let me briefly explain it to you Next, we are going to look at how to implement

our final allocation and deallocation right? Now, when drawing this diagram, Because if everyone draws it this way it looks rather awkward. Because as we know, in reality there is a correspondence here. As we just mentioned, that is to say, this thing

this thing actually corresponds to this one here, this one. And then the second one corresponds to this one here, the column group, the index. So drawn this way, it looks rather awkward. So later, when we draw our diagrams, we'll use a different way of drawing it.

It's just a different way of drawing it. We'll take that memory in the front that's used for management, and draw it at the top, and put the actual memory at the The memory allocation goes down here. Now, this one, it's actually not.

This doesn't mean there's a pointer-linked list. What it actually means is just there's a correspondence. Because we said, if we use the array approach, we're actually avoiding a linked list. That is, no pointers at all. For us, it's really just an

index-based relationship. Because its this region, its index in here is 0, and the index of this region is also 0, right? Through this index, it's actually quite easy. work out the correspondence between them. So that means, there is implicitly a pointer,

just like it, but it does not mean there is actually a pointer there. Let me explain this to you. We will base ourselves on this kind of diagram to introduce to you the allocation and free algorithm below. So, now that we have this concept,

first of all, we need to design some related important data structures, right? When we write code, we generally design the data structures first. The functions are actually relatively secondary, right? As for the data structures,

it is really about our the internal management of this part here So how should we design it? How do we use it to describe this stuff? Now, as for this thing, as we all know, the first thing we need to express is this: we need to mark whether this region is in use or not,

whether it has been allocated or not. So therefore, we can define a page, and note carefully, this page is used to describe this information. This stuff over here is what gets allocated to the user for storing data. So this page doesn't refer to this thing.

It actually refers to this thing. It's this thing. So inside this page, we can define a flag. For this flag, for now we only since we only need to indicate Whether it's allocated or not allocated So we can use one of the bits in here

To save a bit of space Use one bit to represent it If it's 0, that means This represents its first bit The first bit That is, bit zero That is its bit zero Used to indicate whether it's allocated or not allocated But we say that this is actually not enough

Why is that? Because sometimes, as we said, a region may be contiguous, and we need to represent whether it is or not. We said it might be allocated as a contiguous block of multiple allocations, so how do we represent a contiguous allocation?

We can actually use one more bit to represent that. This diagram might not be drawn very well. But just imagine it. Suppose this one is also red. Then this whole region might be allocated to the user as a single unit. So besides marking it here,

to show that it is in use, red meaning it has already been allocated, we usually need to mark it out, because the address 70 that we allocated to the user is right here, right? Address 70 is right here. So how do we indicate that this entire region is one whole block?

Then we probably need to mark it here, that this thing is its last block, right? It's the last block. So since we need to represent this information, we need to use another bit of this flag to indicate whether the current descriptor

is the last block of information for the whole region. So, with that, we've basically designed the structure to look like this. This part is fairly simple, so we'll go through it a bit faster. Mainly, you can go back and read the code yourselves.

Now that we have this information, it comes down to the actual implementation. For example, let's say we want to implement this allocate function. The parameter it takes is the number of pages to allocate, the number of contiguous pages.

So let's assume that here we pass in 2. Now, if you look at this diagram, it basically means that what we're really doing is looking for a usable region. The red color indicates what's already been used up, so what's available is

these blue parts. So here, actually, this block is the part we can use. Because here, there are two consecutive blue parts, right? So let's see, how do we find this region and bring it out? Well, it's actually quite simple. Once we have this,

after we have the start information and the locate start information, we can actually, and once we've defined this data structure, we can first set a pointer starting from this start, and then search backwards from there, looking backwards.

So we search, search for what? So we go find its basically in the management structure at the front, within this chunk of managed memory we go look for the parts that are blue. In practice, it's really a check of whether the corresponding 'taken' bit

is 0. And once we find it, once we find the first available spot, we effectively get its actual starting position. But at this point we can't be completely sure yet, because as everyone knows, it could be like this situation, where just one is not enough, right?

So we need to use another pointer to check the part after it, whether it's still, consecutively, still blue. So in this case, this way Only then can we finalize it. This region then returns the pointer. So once it's returned, it essentially means that

this chunk of memory is fixed. We hand it to the user, and the user can start using it. This is allocation. This part is quite simple. Deallocation works in a similar way. So if we want to take a take a pointer, and then free it,

what we actually do is, combined with the structure we saw earlier, we have to start from this because we can calculate it, based on this position, we can actually figure out this position, right? Think about this position. After we get this address,

why can we compute this position? Because we can compute this offset, and then based on each of its configuration sizes being 4K, we can work out that its interval is 3. So this 3 is the index. And then this 3, based on this mechanism,

we can compute this position. Right? Once we have this position, it's easy. It's essentially just saying, we won't do a live demo of this for you. You can look at the code yourself. It's about releasing. Releasing is essentially just saying,

you have to look backwards, find each of its contiguous blocks, and keep going until you reach the last one here. You find it through this last flag. until you find the part that is not a single blue color then turn each of them back into blue

and just release it so, everyone we'll leave this function for you to look at on your own so as for this, basically page allocation that's all we'll cover for now so as for memory management on RISC-V there are actually some more complex operations

for example, memory protection and virtual memory as for memory protection it essentially means we can set for our memory region we can configure separately whether it is readable or writable these attributes because we know that when we

in a section to describe it as readable and writable, that doesn't actually give you real read/write control over memory. The real control actually has to happen at the physical level. So we actually need this, but this feature requires support from the underlying hardware.

That is, the question is whether this CPU supports this kind of PNP memory protection mechanism. And by using this mechanism, we can actually achieve the effect of controlling the read/write attributes, and the executable attributes of a specified region.

We won't cover this part in the course material. If you're interested, you can look at sections 3.5 and 3.6 of the RISC-V privileged instructions manual. Another very important topic is virtual memory. As for virtual memory,

since time is limited in this course, we didn't implement the virtual memory part. So we won't cover it in the course material either. If you're interested, You can take a look at that. Sections 4.3, 4.4 to 4.5 on privileged instructions.

Because once you introduce virtual memory, you can actually implement more complex things, such as the concept of a process. But for this small operating system of ours, as I mentioned to you earlier, we only implement up to the RTOS level.

Just be aware of that. So that's all for memory management for now. After class, you can work on 8.1. That is, on top of the configuration we have, we can implement one with a finer granularity. Implement one yourselves, that malloc and free,

with your own granularity. to handle memory allocation and management I think this one doesn't really involve much of the actual low-level architecture knowledge of the architecture they can just do it themselves through purely software approaches

so I won't go into that here
