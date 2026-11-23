# English Narration Script

All right, let's continue.

So far, we've mainly been introducing some basic concepts of computer organization and instruction set architecture. From here on, we're mainly getting you prepared for the hands-on work of developing an operating system later on.

So, Chapter 3, is mainly about some concepts related to compiling and linking. Let's take a look together. For this part, we'll split it into two main sections. One is an introduction to GCC. And the second part will introduce some concepts related to the ELF file format.

Let's look at the first part first. For GCC, we'll start with a brief overview. First, what exactly is GCC? Second, we'll look at its command-line syntax. Third, we'll mainly cover its execution steps, as well as the file types involved.

Now, some of you may have already seen some of this content before, so you can also treat it as a review and go through it together. So GCC, as we say, is actually an abbreviation for the GNU Compiler Collection. And GNU, as we know, is actually a

organization for open-source free software. Under this organization, it has developed a lot of tools. GCC is one of them. Now GCC, it is actually a since it's called a Compiler Collection, what it really means is that it is a collection of compilers.

Collection just means a set. And since it's a collection of compilers, that means it actually supports the compilation and linking of many programming languages. So this, these programming languages, the most important one is C.

Originally, it mainly supported C. Then later on, it was gradually expanded, and it came to support many languages. So not only can it support multiple languages, it can also support multiple architectures. For example, X86, ARM,

and also including our that is, it will support the RISC-V architecture that we use. Right now, GCC already supports it fairly well. So later on, when we compile and link our programs, what we use is actually this GCC development toolchain.

Now, this GCC toolchain, is mainly installed and runs on Windows No, sorry, I misspoke. It mainly runs on systems like Linux, which are called UNIX-like operating systems. The so-called UNIX-like systems, that is to say, these operating systems

have essentially all imitated the early interactive interface of this UNIX. And GCC, I should say, is currently also, on these operating systems, you could say, a rather official standard, a standard, and it's used quite a lot.

Now, as for this compiler, let me digress for a moment, that is, after our environment is set up, uh, well, uh, you, that is to say, some of you came back and asked me, saying that thing is put at, how do you do it? That is, after we install it,

you can just take it For example, there's an archive file tools and you can download it It's an archive called tools After you extract the archive, what you get is actually inside this Tools directory Inside this directory, we actually have two subdirectories

One is the GCC directory and the other is the QML directory Our build tools are placed under the GCC directory So all you need to do is add this path to your bashrc file or your profile file, and then you'll be able to find these tools

Let me just mention that briefly. Now let's quickly review how to use GCC. The GCC commands are all command-line based Its command line, it's called GCC followed by an option. The options are the ones listed below, options that start with a dash.

And then you just follow it with the input files you want to compile. Now, do you all still remember these options? Let me just ask you quickly. Do you remember what -E is used for? Let's recall this in passing. What is it used for?

It's used for preprocessing, right? What do we mean by preprocessing? It means that, for example, taking C programming as an example, at the beginning of C code there are a lot of things, things starting with a hash mark, like include,

define, and so on. We call it macro assembly, not macro assembly, it's macros, right? So preprocessing, then, is really just taking those macro definitions and turning them into what? Turning them into standard C code, that's all there is to it.

Now, GCC is the most widely used one. And as we say, it's compile without linking, right? So-called compiling, basically means turning this C code into what? Into machine instructions, right? Yeah. That's compile only. Uh, the idea behind linking,

basically is that when we finally want to generate an executable program, we need to take it, take this .o file you've generated, which is machine instructions, and link it together with some library files to link them together

that's the concept of linking and this capital S -S, do you remember what that is? the capital S the capital S means generate assembly right? So what that means is, earlier on when we wrote the lowercase c, we were actually skipping this step.

But this step is actually something that still needs to happen. Because the whole process is really first turning the C into assembly, and then turning that into machine instructions. Right? This, we covered in our very first class,

when we talked about the evolution of programming languages, almost like the evolution of humankind. This is actually the process of human evolution. Actually, from the time you develop as an embryo to becoming a small child,

you go through a process like that too. And our compiler works the same way. In a single step of yours, it actually still contains all these steps. Now, -o specifies the output file, directing it to a filename you choose yourself.

The o stands for output, right? Now this -g flag, as you'll see later on, means that if you want to do debugging, you must add -g at compile time. That way, it will include some debugging support information in the generated file.

So our program, only then can you debug otherwise you can't Now what about -v? It means verbose. In other words, during execution, it can display some of the detailed steps as it goes along. These options are the commonly used ones,

and I hope you all master them thoroughly. So as a whole, when we run a GCC command, what actually happens behind the scenes? Well, we need to understand some of the basic concepts about compilation, assembly, and linking. So we can actually go through this process,

and we can take a quick look at it. Let me see if there's a ready-made program here. Let me check if there's anything here. If not, we'll just write a simple one. Let's simply create a the simplest hello. Writing this now should still be

since for this course, C is a basic requirement, we won't say much more about it here. So, suppose we write a simple hello world in this demo directory. Now, we say, with GCC, for example, if I want to compile this thing, we just type gcc,

hello.c, and that's it. And then by default it will generate an executable program called a.out, right? If we specify -o, But, if we want to see the detailed process, Actually, it's very simple. We can at compile time, add a -v So after adding -v,

you can see that it will output all the detailed information about the compilation process. Now, this detailed process actually contains what I want to show you in this diagram, namely the entire compilation process. So the first step,

we call it preprocessing. That is, it's equivalent to saying for this for this .c file, we add a -E, this -E option, and it will run the preprocessing step, to execute all of the things in here that start with a hash sign — the C preprocessor directives —

and turn them into C code. Now, here we've added a -o, and by adding this -o here, we're actually specifying that we output a file with the .i suffix, a file with the same name. Now, as you all know, once this file is generated,

after adding this -E option, what ends up in the generated file has essentially had all the preprocessor directives removed, so what's left in the .i file is one hundred percent pure C code. That is essentially how it works.

Now, at this point, our next step is really what we call the compilation stage. Now, in the preprocessing step earlier, it's actually done by a program called cc1. It's a small program that's part of the GCC development package.

This compilation step, it's carried out by the cc1 program. Now, besides doing the preprocessing, cc1 also performs the actual compilation process for us. So the compilation process is basically taking C code, and converting it into assembly language.

So we can also run this command on its own, to see the effect. Now, this process, essentially across these steps, what's generated at this point is assembly language. So once we have the assembly, the next step is to turn that assembly language into machine instructions.

Now, this step of producing machine instructions is done by a program called as, which is the assembler. So the last step, we said we generated this .o file, but it still can't be executed directly. If we want this program to run,

we actually still need to link this .o together with some standard libraries. And this linking, is actually carried out by another program called ld. And only then, an executable program is produced. In other words, this whole...

when I type a gcc, what actually happens behind it is such a complex process. And this complex process, we can actually see it in what we just showed you. Take a careful look at this process, and you can actually see this process taking place.

Where is it? Let's take a look. Let me find it. Here I see an AS. And AS is what we just talked about, this assembly thing. Let's redirect this into a file. That's all there is. This doesn't seem very clear. Let's take a look. It should be visible here.

Add -v. AS this one at least it just See, AS generated this .s file. Now, this .s file, it should have been generated in one of the earlier steps. Let me find it. Can we see it? Here, you can see its first step actually calls cc. Do you see that?

Right? This cc, on its side, will generate a it generates a temporary file, called a .s file. Now, this generated .s file, this process actually contains what we call preprocessing and compilation. Then, after generating this .s file,

it takes this .s file, and continues by calling the as command, right? This as command, is what we call the assembly stage. Now, what will it actually generate? After -o, what does it output? It outputs a .o file. This .o file is actually a

file containing machine code instructions. Now, for the last step, you can see it actually will go on to call which program? For the last step, it actually calls a program called collect2. If any of you are interested in looking into this program,

what it actually does internally is wrap our LD program, that is, the linker. Inside it, you can see it actually will take the .o file we just generated, and our .o file, and our .o file, what was it called again just now? Where did it go?

There's quite a lot of typing here. I can't seem to see it. The .o file should be right up here. Here it is. This .o file is actually down here. It's just a part of what's inside it. Let's take a look. Here. It actually does something this complex,

such a an operation. Let's copy it out and take a look. So in this process, you can see for this collection2 program, it actually the .o file should be here. Right here. The one generated after our compilation, that .o file is actually in this spot.

The font might be a bit small. Can everyone still see it clearly? I won't adjust it now. If you're interested, you can go back and take a look yourselves. The thing is, this .o file, actually, the .o file we generate, it's right here.

But during the linking process, it actually links in a lot of other library files. For example, as you can see, it specifies a lot of these .o files here. These .o files, they're actually standard libraries that come with our GCC.

So these standard library files, along with our program's .o files, get assembled together. That's how our executable program is finally produced. That's actually the process. So throughout the whole GCC process, a very complex process actually takes place.

You can take a look at this when you get back Now, sometimes we might want to break this process down and inspect the assembly code generated along the way and the machine instructions In that case, you might need to run these commands individually

to generate those assembly instructions and machine instructions yourself These are the main execution steps of GCC As for the file types mainly involved in GCC we already saw some of them earlier That is to say, it may involve some

for example, different files are distinguished by their filename extensions If it is .c, it is a C language source file If it is .cc or .cpp or .cxx then it is probably it should be a C++ source file The .i file I just showed you

This one in here is the preprocessed source file. Now, what about the ones with a lowercase s and an uppercase S suffix? Those are both assembly language files. Now, some of you might ask, why are assembly files split into a lowercase s suffix and an uppercase S suffix?

Well, here's the thing: if the suffix is an uppercase S, then generally that kind of assembly file actually also contains preprocessor directives, for instance things that start with a hash sign. Whereas this .s file only contains

100% pure assembly instructions, that's how it works. Then .h and .o, I won't go into those. Object files. Then there's also the common .a, which is a static library file. It's short for archive: .a. So .so is shared object meaning shared library.

So a shared library file is actually somewhat similar to a dynamic library on Windows. So the executable file, by default, is called a.out. Of course, we can also specify an executable file ourselves. These are some of the common file types we see in GCC.

So just now, what I showed you was just the compile and link process for a single source file. Now, if your project is more complex, and maybe contains multiple source files, then the build process, compared to before, is slightly different.

It's just that you need to run this process for each individual C file separately, so for each .c file, the process is actually the same as before. So for each .c file, Each one will produce its own .o file. Then, at the end, through linking,

we string together all the .o files and some standard libraries, and that forms an executable file. So, when you have multiple source files, the overall process is roughly like this: for each source file, we generate one .o file.

First, we compile, producing a .o file. And then, finally, we use gcc, and this command, as you know, what is it actually doing behind the scenes? What is it doing here? It is actually doing the linking. Right? Earlier, because we included a

The -c flag tells the compiler so over here, it only compiles and does not link. So what it produces is a .o file, an object file. These are all basic concepts, and I think if you all have worked on Linux or, let's say, used it for a while,

none of this should be a big problem. As for GCC, we'll just quickly go over it and stop here. Now, in the work that comes next, we will actually need to often analyze the things we generate — that is, The contents inside a.out or .o files

Now, these files actually, as we say, they all have their own format. They are not just simple files. This format is called the ELF format. So what exactly is the ELF format? And what is the specific built-in concrete form of this format?

And how do we handle this kind of ELF file? In this chapter, we will give you an introduction. Now, the word ELF, as a matter of fact, is the abbreviation of these three words, that is, executable linkable format. And translated into Chinese,

it means it is an executable, linkable format. Right? We, for these files, All we're really doing is linking or running them. So in short, it's called ELF. So it is, on all the UNIX-like systems, a kind of standard format for binary files.

It's extremely important. You could say it's a very fundamental concept in our computer industry. So this, on our UNIX systems, including Linux systems, its ELF format, there are mainly four kinds of files that all use this format.

So let me also mention this here. First, the ones generated during the compilation process we just did -- those .o files, we sometimes also call them object files. right, the object file. And in this ERF... I mean this standard specification,

we actually call it a relocatable file, called relook... er, how do you pronounce this word? a relocatable file. And what relocation mainly means, is that there are certain addresses that during the compilation process, can't be fully determined right away.

They need to be worked out during linking, and that address gets pinned down there. That's the idea behind it. We probably won't go into detail on this here, but if you're interested, you can go read some books. For example, I'd recommend,

let's see, there's a book called The Craftsmanship of Programmers, which is specifically about compiling and linking. You can go and take a look. Then the executable file we generate, say a.out, this a.out, the executable file also belongs to,

it uses the ELF file format. And then there are .so files, which are what we call shared library files. This file, it can, at run time, be dynamically linked, unlike before, where we generally, the most common is static linking.

It can, at run time, be linked with executable files. And these shared library files, also use the ELF file format. And the last type, is the .ko file. I don't know if any of you have come across this file. That is to say, when

for example, a program crashes, it might Of course, this requires us to configure it in the system. By default, it's not generated. But once you've configured it, you can specify that when a program crashes, it can generate a core dump file.

Now, inside this core dump file, it contains a lot of information from the time the program crashed— on-site environment details, which can help us figure out what exactly happened on the system when the program crashed. Now, this kind of core dump file

uses the ELF file format. So that's why this ELF file format is extremely important. If you are optimizing programs, debugging and troubleshooting them, Some of the more complicated problems, they may involve okay, this, this basic concept.

So what does this ERF file format actually look like? We've drawn a diagram here for everyone. You can take a look at it. This ERF file, first of all, it if you just use VI or a text editor to open it, you won't be able to read it clearly,

because it's a binary format. But it actually does have a structure. The first part of it is called the file header. Inside this file header, it contains some basic information about the ERF file. For example, this program — which architecture is this file meant to run on?

Because as we know, with different architectures, the information inside this ELF, it could be different. So it needs to, in this header, interpret that information. This is the header. And then there's some version number information,

all in this place. Now, right after the ELF file header, in the part below it, there's something called the Program Head Tail. We'll talk about this later. Let's first look at the main body in the middle, which is the white part.

This part, it's a series of one after another... We call it a section for example, this one of ours, what's placed inside it is different types of content. For example, .txt, inside this one, it holds a bunch of program instructions —

that is, the executable program is placed right here. init, well, is probably used for some initialization instructions. data, well, probably holds some data — for instance, some of the things we define in the program, global variables.

It's placed right here. These concepts. I don't know if everyone... Whether there was a course earlier... Whether this was covered... Whether this was covered before... This part of the content. It was covered, right? If you've covered it...

It'll probably sound a bit easier to follow. For us here... Since we're going through it quite fast... So this... So in this file of ours... What sections are actually in it? It's actually through... The blue part at the end here...

which is called the section head table... that it's described. As for my blue line here... It basically expresses the idea that if we look at the section header table, we can find out what sections exist in this file, what each of these sections

means, how big each of them is, and where it is located. All of this is described in the section header table. So, we can think of it this way: this section header table, this part, actually describes, describes the contents of this file from a linking perspective,

because linking involves this part of the content. Now, these parts, we call them sections one by one, which is the concept of a section. In Chinese, we call it a section. OK, so now let's come back to what we were just talking about,

this red part at the front, which is the program header table. What information is it actually used to hold? And why do we need to introduce this program header table? Well, it's actually a kind of... what we call... it actually describes a

concept called the run-time view. So what is the run-time view? It means that when we... the file we ultimately generate, we know that this file, let's say it's an executable file, then this file, when we finally run it, it has to be loaded somewhere first, right?

loaded into memory Think about it, isn't that right? So during loading, these sections, if we don't distinguish between them and just load them one by one, that is, load each of them in separately, that could introduce a problem.

What problem? Well, as we know, our operating system, when loading these sections of ours, in general, will align them to a certain width. What does width alignment mean? For example, if it aligns them at 4K, then that means, say this .txt section,

say it takes up 40 bytes, it will still occupy a 4K-sized region. for the rest of it, it won't put anything else in there. Then the next section, say init, it might have to start being laid out from the next 4K byte position.

So think about it. So, you see, in that case, if we always do it this way, then when these are put into memory, even though my file is not big, once it gets unpacked into memory, it might take up a lot of memory. That's quite a waste.

Do you all get what I mean? So what do we do? Well, when we actually load it into memory to run it, we actually take these sections and merge them together, and when we merge them, these sections are merged based on their attributes

For example, we will see that in both the text section and the init section, what's stored is executable program instructions. So we can put them together. Think about it, doesn't that make sense? So by putting them together,

we merge them. This curly brace means they are merged together. After merging them, we call the result a segment—that is, a segment. When we actually load it into memory, the loading is done in units of segments. Each segment may contain one or more sections.

The main purpose, as I just mentioned, is to save memory. Otherwise, the wasted space in between would be far too large. So, that's why we take this pro Which of these sections go together how big each of these grouped segments should be

how much space they take up and exactly where they get loaded The description of all this information is all specified in the program head table Now, in this table, we call it the run view and that's exactly what it means It's because the information described in here

is only used at runtime if you see what I mean That's why it's called the run view So, our entire ELF file consists of these major parts and they each have their own meaning Now, things like segments and sections, we'll run into them a lot later on

In particular, I think when you run programs, you'll often run into the famous segment fault Now you know why it's called a segment fault instead of a section fault Why is that? Because when you get an error a failure in memory

he understands what I mean So this is All of these things come from these basic concepts So this is the basic format of ELF and it's a key focus of our study As for more detailed content you can probably go and check some reference books

Now these ELF file formats when it comes to these files, if you asked us to manually analyze the contents ourselves it would be extremely tedious So GNU also provides a matching set of related tool programs. And these tool programs

are all bundled together in one toolkit called Binutils. It is actually bin, b-i-n-u-t-i-l-s, which is actually an abbreviation of two words. So "bin" stands for binary, as we know, that means binary files, right? U-t-i-l-s stands for utilities,

meaning tools. So the full name of this software is actually Binary Utilities, that is, binary tool programs. Inside here is really a software toolkit, which contains a lot of small programs. Each of these small programs has its own specific purpose,

and we will touch on them later. So let me also introduce to you here some of the commonly used ones For example, AR, which we also often use, is called TAR, which is actually pretty much the same as AR. Then archiving is just packaging,

it's packaging, right? For example, when we want to generate a static library, we need to use this little program AR. Then AS we talked about earlier, AS and LD are actually in the binutils toolkit, not inside the GCC installation.

Please take note of this, because GCC actually calls the ones in there indirectly afterwards. Then this OBJCOPY, and OBJDUMP are used for file format conversion and displaying contents. We use OBJCOPY quite a lot, and we will use this objdump extensively.

As for readelf, it will also be used quite a bit. What it does is let us look at an ELF file whether it's a .o file or a .out file To see the detailed information inside it we use this program to look at it Later we can show you a demo

of some basic uses of these tools Let us show you the basic usage of this one And perhaps along the way we can walk you through a small exercise You can take a look In the code that you downloaded there is an exercise including when we assign homework later

it will all be in this exercise Under the slides directory Under this slides directory there is one called exercises the exercises PDF You can go ahead and open it and take a look. The exercises for our corresponding chapters are right down here.

It's a PDF file. For example, in Chapter 3, there are some exercises over here. So now let's go through them together, and do a quick run-through of Exercise 3.1. So let's basically get familiar with using these tools. So first, let's

write a simple program file that prints hello. I think I've already written this one. I'll just put it right there. And then, if we want to natively compile the source file, to generate an object file targeting x86 instructions,

well, what do we mean by native compilation? We'll come back to that later. For now, let's not worry about it. Anyway, we know that at least for now, if you haven't done much embedded development, you've probably just run GCC locally.

Now, that's called native compilation. So if we want to do native compilation, we know that if we're going to do native compilation, how do we actually do it? Based on the concepts we covered earlier, let me close this first.

Don't save. So, I now have this hello.c here. Now I want to... What we're going to do now is, let's all follow along and get familiar with the process together. If you want to do a local build to generate the object file for it,

what does it tell us if we want to generate an object file? We just compile but don't link, right? So how should we do that? Don't we just run gcc -c hello.c and that's it? Isn't that right? Once you hit enter, by default it generates a .o file with the same name.

Of course, you can also specify the one you generate. If we write it out in full, it should look like this. Right? Of course, you can leave it out too. If you leave it out, it defaults to the same name. If you want to change the name,

say you insist on calling it hh.o, then you write it like this. Isn't that right? I won't write this out. So this way, we've generated a hello.o Now, as we know, what's contained in this hello.o is definitely all machine instructions,

right? So let's say we now want to do what? The next step is for us to look at the file header information of hello.o. What do I mean by file header information? It's what we just looked at, the green-colored part, meaning we look at this part of the content.

What method should we use to look at it? If I asked you to look at it, can anyone tell me? Are you still familiar with this part? I don't have your name list here. As for the name list, next time I'll ask you for a name list, so that I can call you by name.

Can anyone step up and tell me bravely? It's very simple. Which tool would you normally use to look at it? Some of you are saying objdump, right? Actually, using objdump... they still don't use readelf, because the name readelf is too... readelf-y,

it's read-elf, it just means to read it, right? Everyone, go ahead and give it a try. Actually, readelf's viewing capability is a bit stronger than objdump's. Otherwise, why would we have objdump and then make another readelf? Now, with this readelf,

we want to look at the header. For the header, if you're not familiar with it, you can also use the man pages. take a look at this thing there will definitely be detailed content introduced on there so I won't walk you through it here

then reader1lf followed by -h h stands for head it's an abbreviation, right? here it doesn't mean help then we follow it with hello.o and you see this header all the information inside gets printed out but if you just go and use vi to look at hello.o

you'll see a bunch of garbage characters in there you won't be able to make sense of what's inside you all know that, right? so we definitely have to use this tool to look at it now in here, as you can see it tells us some information

First up is magic. Magic is called magic number. This magic number, honestly, it's not of much use. Its main purpose is just to basically, give this ELF file, give it a special marker to distinguish it from other files. It doesn't really carry much meaning.

It's not very meaningful to us. Then class tells us the file type. We can see it's ELF 64-bit, which means it's a 64-bit file. I won't go into the rest in detail. As you can see, it gives a lot of information inside. Now for our next step,

we want to take a look at its section header table. What is a section header table? That is, this part in blue, this blue part, we know what kind of information it describes. It's a link view, describing what's used for linking.

When doing local linking, how many sections are there inside, right? What is each section called, and where are these sections located? It's basically that kind of stuff. So how do we look at this linking, the linking? Well, we have a...

let me clear this first. S just stands for section. You see, it's just a bunch of stuff like this. But when you look at it this way, it's rather hard to read, because it wraps a line over. It makes sure the overall width isn't too wide.

Now sometimes this looks a bit uncomfortable to us. So we can add one over here. Add a W. And then it looks comfortable. It's actually the same thing. It's really just making it wider. W just means wide. Now you can see, what it actually lets you see is,

you can see, in our entire file, how many sections there are. Sections with text. Over here is the section name. The section number. And then the section type. It will tell you what these sections are. For example, this thing, it says PROG.

It's actually PROGBITS. That is, programmed bits. Which basically means it is an executable program. That is actually what it means. And this is the relocatable stuff. If any of you are interested, you can go take a look. I won't say more about it.

Then there are some address information inside it, the size, and some flags inside to mark some special purposes of these sections. So we can take a look at that. Then we can also and then, finally, let's do a quick exercise. For example, suppose we want to look at the disassembly.

Suppose we want to look at, because there might be some scenarios, for instance, when you are programming, you only have an executable program in hand. But if you want to know what exactly is going on inside this, then in fact,

if you can get the source code, you can see very clearly how it works. Of course, if you can't get the source code, you only have the assembly, then you just have to look at the assembly. Right? Reading assembly is pretty painful.

But if you can get the source code, we'll be able to see the correspondence between the assembly and the C code, and that's much better. So if we want to disassemble it, first we need to be able to take that program and add a -g flag to it.

Let's delete this hello first, because this one doesn't have debug information. So we need to add a -g to this. I wrote that wrong. The key thing is that we need to put a flag here, and that's it. That way it includes the debug information.

So compiling this is still the same as before. Now, what we want to look at next is, to look at the return value, we'll use the objdump command. So for this objdump command, we pass it a -s and then

for the .o file,

you probably still want to just press Enter first, it might be too long. It can show you as you can see, in this case, it will actually show

its binary, over here is the return value, it writes out the corresponding assembly instructions. On the right side here, those are actually the assembly instructions. At the same time, and with -s, we can still because if we get the source code,

we can actually map the source code correspondence and get it that is, essentially, these two statements below, they are actually corresponding to this printf statement, and on the right are its assembly instructions, right? So this is to say,

I'll just give you a brief demonstration of these, so you get a feel for how these binary tools are used. Now, as for compilation and linking, we went through that a bit quickly. But if you really learn compilation and linking well,

there are actually a lot of benefits. I won't go on and on about it here. If you have a bit of programming experience, you should already appreciate that. That is to say, these things Although we don't cover it much in this course

and including at some schools, the coursework doesn't go into this area much either, but in actual work, understanding ELF and the concepts around compilation and linking is really a huge help when it comes to debugging your programs

and optimizing your programs. It genuinely makes a big difference. It also helps a lot with writing programs that are stronger more robust, and more resilient. It's really quite useful for that too. So that's it for compilation and linking

— we've covered it quickly for now.
