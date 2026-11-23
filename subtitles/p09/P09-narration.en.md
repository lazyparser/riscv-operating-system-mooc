# English Narration Script

Let's start with the first part: arithmetic instructions.

Arithmetic instructions, in English, are called Arithmetic Instructions — that is, arithmetic operations.

Now, they are nothing more than addition and subtraction. Note that this does not include multiplication and division, because those instructions are not part of the base integer instructions. Multiplication and division have a dedicated M module,

that is, the multiplication and division instruction set, the extension set and extension module, which we will introduce to you. They are not within the scope of this part. What we mainly deal with here is addition and subtraction.

The first instruction we are going to learn, is the add instruction, which is addition. This is the most basic one. Now, this add instruction, its encoding type is R type. So this R type is the register type. What does register type mean?

It means that its operands, its three operands are all registers. You can see it here. In terms of encoding it needs to express three registers RD, RSC, R2 Now, when translated into assembly

this is the syntax format Note this in RISC-V the assembly the convention is to put RD the register that finally holds the result first reversed that is, the sources go at the back the two sources at the back there is such a relationship in there

within this relationship when it reads what it means is this that is, when doing addition How do we add? We take out the value in this first RS — the register inside. What I'm explaining right now is actually from the CPU's point of view.

If I were the CPU, when I'm executing this instruction, how would I carry it out? I take a value out of RS — that's one of the addends. Then, I take another value out of RS2, and I add these two values together, getting a result.

Now, this result — where do I put it? I put it into the RD register. That's really all it means. This is the add instruction. Now, when we actually write it, we usually use lowercase, when we're programming. Normally we use lowercase

when we write this syntax. I deliberately used uppercase, mainly because it looks nicer. For example, x5, x6, x7. We talked about this earlier. Each register has a code name, right? So that's 5, 6, 7. Its encoding. As for the specific encoding,

it's actually quite simple. If it's x5, you can imagine that the value inside those 5 bits is just 5. The value in x6 is 6. Once you understand that, you also need to be able to read it the other way around. Now, this encoding format is R-type.

The opcode is 011 0011. Why is it this value? Just look it up in the table. It's listed right there in that table. I just demonstrated that briefly earlier. Right? And then, function3 is set to 000. function7 is set to seven 0s. Now why is that?

Actually, we may have gone through this a bit quickly just now. Let's go back to that table again. If you look up the add instruction in this table, it's written out very clearly here. The add instruction is exactly like this.

This is the opcode field. This is function3 in 3 bits. This is function7 in 7 bits. This is the first instruction. So it uses the simplest encoding. All zeros. That is to say, these three values uniquely determine that this is an add instruction.

Pay attention to this, everyone. Uniquely determined. This is an add instruction. Then RS1, RS2. We just covered this earlier, the concept of source. RD is the destination. It's used to store the result. Very simple. This is an add instruction.

Now, let's briefly use this instruction as an example again and take a look at how we Once we've sorted that out, what does the final encoded machine instruction actually look like? So just now, by looking it up in the table, we already know

the OPCODE is 0011, and these were found from the table just now. That leaves RSE2, RSE, and RD — these three fields, and we still don't know what to fill in, right? So let's assume our command looks like this. Based on the explanation we just gave,

we said 5 corresponds to D, 6 to R1, R2. So what does that tell us? It tells us the value placed in this RD is what number? It has to be 5. Think about it, right? What's stored in that RSE? It's definitely 6. Then 7. S27. So, but ultimately, what gets stored in the computer,

is definitely not an integer, not a decimal number. It definitely has to be turned into something — what is it? Binary, right? Now, binary, you know, that is, let's convert it ourselves. Do the conversion. This is very simple, right?

It's just 111. The moment you see it, you should know it's 7. Right? For example, in that command line of yours, Type that, what is it When you change mode What does that 777 mean Right? Isn't it just 111? 111, right? Isn't it? These numbers should be somewhat

familiar to everyone For example, that change mode What does 664 mean? What does 777 mean? Right? Of course, I'm getting off track here So once we have this string of numbers We basically know What's in this memory is just this thing

But ultimately we still need to know where it is in memory How are they laid out? Right? So usually we'll also take this binary, which looks pretty ugly, and for most of us, we like to group it in eight bits, eight bits, and split it up,

and then finally convert it into a hexadecimal number so we can work with it, so we can do this, to read it, or to manipulate it, right? in programming. So in the end, we know that this add x5 x6 x7 assembly instruction, let's say your assembly instruction,

then after it's processed by the assembler, It's essentially something like this: a binary number. Now, with these binary numbers, how are they laid out in memory? This brings us to another issue: the issue of endianness. Do you follow what I mean?

Now, conversely, say I give you a piece of machine instructions, you can basically disassemble it back. There's no such thing as 'discompiling'... you can still disassemble it. So this is the most fundamental skill of a hacker.

Through this, we get to understand how instructions are encoded, and how they are stored in memory. So next, we've covered this part, so let's look at an exercise. This is our first one for today, the add exercise. Let me walk you through it once now.

Our exercises, as I mentioned earlier, are all placed here. Each directory, basically, is the directory for that command, the name of that command, like add. So for ours, under each directory, let me briefly go over the file organization.

It's generally one source file, plus a makefile. This makefile, actually, the makefile in this first set, you can't really see its actual content. It actually works through It's just defining some common variable names. And then,

the key point is here, which is include. This make syntax, everyone is familiar with it, right? What does this include mean? It will actually look for this file, then expand this file, and substitute it here, just like macro expansion,

right? Like preprocessing. Now, this root.make, where is it located? It's in the parent directory. So to actually find this root.make, it's very simple—you just go into this asm directory. Because right now we're in this add directory,

we need to go up one level to the asm directory. Once we're in the asm directory, actually right here there's a root.make. Inside this file is really our actual make statements. In the last class, when we covered the makefile, I think I briefly walked everyone through this file,

right? I don't know if you see any issues with it now, or if there are still questions. Actually, when we do the exercises, we also demonstrate them based on that QEMU emulator. to demonstrate for you all And since we want to see how the assembly instructions are executed,

so don't we mostly run it directly Because if you just run it, you can't really see any results. In practice, what we use more often is the GDB debugger to look at some state in memory. So, when you do the exercises later, you'll probably need to use

the make debug command more. As for the make debug command, what it actually does inside, basically, it starts up QEMU, and then, it takes the file we compiled and generated, this file, and through the -kernel parameter, it loads it up.

At the same time, please note, there's a -S here, a capital S. This one, Let me also briefly explain what QEMU actually is over here since we haven't covered it yet QEMU, it's actually this thing right here Last time I probably went through this part a bit too fast

and just glossed over it in one go so let me go over it again for you This thing here actually serves as a system-level emulator for QEMU Because if we look back for a moment, you all remember, right? QEMU has two running modes

One is called user mode We generally use user mode to run a executable program The kind we're running now, compiled from assembly instructions, strictly speaking, It's actually a piece of logic code. It can't be run on an operating system

directly. If you try to run it, directly under an operating system, for example, running it directly on Linux, it won't execute. What we're actually doing is running this code directly in a logical sense, so we launch it in system mode.

So that's why we start it in system mode. system is the 32-bit RISC-V machine simulator, and then we pass in some parameters. -nographic basically means, graphic, as we know, means graphics or images, so it means it doesn't start a graphical interface.

And then -smp means we only start a single core, so basically just one hart. And the machine we start is of the machine type called virt, that is, the simulator. It simulates a device type called VRT. What this device type is,

later when we talk about the operating system, we'll look at it in detail. For now, just having a simple understanding is enough. Anyway, through this approach, we get the emulator running. It's essentially like a hart is up and running.

Then on this hart we run the assembly instructions we compiled and generated, the machine instructions. So this is QEMU. First, we launch a QEMU emulator. And when we launch it, after adding the -s and -S options, what effect does it achieve?

Well, let's review for a moment. We talked about this before, this kind of debugging method is called remote debugging. Because the previous one, I'm not sure Maybe let's briefly review that part again That is to say, on which diagram

On the one with the crossing On this diagram Don't move it On this diagram Everyone still remembers, right? Let's run it That is, we said that QEMU and GDB work together to debug So we're actually getting to today's runtime environment

We're actually using a remote debugging approach Why? Because it's a cross development and debugging environment Our target machine and the machine running GDB You can actually think of them as two different machines It's just that our target machine

is currently an emulator That is QEMU QEMU It emulates a RISC-V machine, while our GDB is actually running on an x86 machine. Right? This is a concept you need to grasp, which is the cross development and debugging environment.

So when we run this QEMU, if we pass the -s -S flags, especially the -s option, it will actually automatically start a GDB server for you inside QEMU. This diagram is explaining exactly that. Between GDB and the GDB server, we need to connect over a network.

It's just that right now, the emulator and our GDB are both actually running on the same Ubuntu machine, right? So in practice, this network connection is a local loopback, not a real one. Of course, you can also actually run it

on a different machine, and that works fine too. For example, you could run this emulator on another machine, and then establish a connection over the network. That works as well. So what does this -S do? It will actually set up a network connection,

and moreover, as a service, as we say with sockets, it definitely needs a port number, right? You still remember that concept, right? Well, with our -S, it defaults to using port number 1234 and starts up a service. You can think of this GDB

right on this port 1234, waiting for GDB to initiate a connection to it. This is basically how the Socket works. So after we launch GDB, we connect to it through the Socket. Once connected, we can debug on both sides. That's the idea here,

which is the effect of the lowercase s. Now, what about the uppercase S? What effect does it actually have? It makes the program, well, once you run it under GDB, it will actually stop there and wait for you. Isn't that exactly the effect we want?

If it runs off on its own, we'd have no way to debug it. Think about it — isn't that right? So if we put the whole thing together, we need to include these two options. Just tell QML to run in this mode, and that's it. Let me explain to everyone

what exactly is going on in the lab we're doing right now. So this is why you see it here, the reason we launch it this way. Now, the next symbol means that we will take the QML program process and do what with it? Send it to the background,

right? Once it's in the background, we can then launch a local client — that's the left part of that diagram from earlier. This client will be loaded on top of our compiled and generated ERF file. And then, just a quick note for everyone.

Now, this -Q option, actually just stands for quiet. You can go check the manual yourselves. It's very simple: it avoids this— you know that when GDB starts up, it prints a bunch of garbage up front, and this gets rid of that, so it doesn't clutter our screen.

And then, let me focus on this -x. With -x, it actually takes an argument that lets it run a configuration script. This configuration script, we've already written it for you, and it's a file called gdb init. In this file, it actually runs a bunch of

the debug commands we use all the time. Because as you know, after you start up gdb, you'd have to type these commands by hand to connect to the server side, especially this target remote 1234, which basically acts as a client connecting to that GDB server.

See what I mean? Having to type them every time is kind of annoying, right? So you can take these commands you use often and write them into a config script, and then at that spot we just saw, right here, tell GDB about it through this argument.

Then when GDB starts up, it will automatically read that config script, and run these commands by default for you. Understand what I mean? That way, it's convenient for us to just... not type those commands ourselves every time.

As for what each command means, I won't say too much here. Especially what B and C mean. Earlier this is the most basic GDB command requirement. This sets a breakpoint, right? You still remember? A breakpoint can be followed by a function name.

We just talked about what this start is. This start is actually what's in our program later. The first file from just now, everyone has seen it, right? There's a start here. As I just said, this start is an address. Setting a breakpoint means setting it at an address.

Right? So this start actually tells us We set the breakpoint right here, which is actually the very first instruction of our program. Now this one, this is for setting the breakpoint, and this one connects to the server side. As for c, it just

lets the program run. continue But when it runs, it actually doesn't really run, it just gets caught at the breakpoint. Then this one is actually set design, design, which actually means setting it to single-step debugging mode,

and it's single-step debugging at the assembly level. What this means is, these few commands are actually just because when we debug, Don't you have to keep checking the registers? Right? Right? If you have to look at the registers every single time,

that can be a bit of a hassle. So what we do is use the display command every time, so that during execution, every time you single-step, it will automatically print out the values of these registers for you. It'll print them out for you automatically.

Because the setup we're demoing to you is not that kind of graphical interface. If it were a graphical interface, you'd probably find it pretty nice to use, you know, the kind where one window shows the registers, which you can watch,

and another window shows something else, right? Right now we just have one shell, so this is the way we handle it. You see, this is also Of course, if you want to do it a bit nicer, you could set up a graphical interface to debug it,

but on our side, we won't demonstrate that here. If any of you are interested, feel free to try it out on your own, and if you manage to get it working, you can share that graphical debugging method with everyone. Here we'll just

use the simplest, most straightforward way to demonstrate it for you. So, regarding this makefile run, that's all we'll cover. So, with that said, let's get back to the main topic. Let's return to our add example. This add example is very simple.

Let's read through it now. This piece of code, can you follow it? Combined with what we covered in the previous class, some of the basic assembly syntax. What are all these things up front? They're all comments. Nothing much to say about them.

Then, over here, what's this called? Try to recall it yourself. What's this called? The ones starting with a dot. What are they called? A directive, right? Is this an instruction? No, it's not an instruction. This is just telling the assembler,

from here on out the part below during the assembly process where does it go it goes into the text section what is a section remember that diagram from our last class what an ELF file looks like right we can briefly review that for everyone

so that you don't suddenly lose your train of thought so this might be this is content we've covered before and it will come up again and again here remember this diagram that is, because in the end we what do we use we use this GNU

we use the GNU toolchain it generates all of this What gets compiled and generated, this kind of executable file, or in other words, the binary file, is all in ELF format. So as for where we put these instructions, which section we put them into,

we can specify that in here. It is configurable. So what this piece of code does, is essentially telling, telling our assembler, and the final, the linker, that for all the instructions below, everything under this, should be placed into that section,

the section called the text section. And then over here, we declare a global function, called _start. It is also the entry point of this entire program. Now this program is actually made up of just a few statements. This is a label

marking the start of our program. Here are two statements. What is this li instruction? We'll talk about it later. Let's ignore it for now. For now, just understand it as basically performing an assignment on the register x6. So li x6, 1

is essentially equivalent to x6 = 1. We'll use this later. I will explain this to you in more detail. But since we need to use it here, I've written it down for now. So these two statements are simply assigning values to two registers.

After restoring the values, what do we do? We do addition. Add x6 and x7 together. Connecting this with the concept we just talked about, these two are registers, after adding them together, put the result into x5. That's all this does.

And then, actually, the main part of our program is finished right here. What is this thing down here doing? Next, you can think of it as just a jump instruction. stop is the address of this instruction. Jump to this instruction itself — what effect does that achieve?

An infinite loop. Actually, this instruction doesn't do anything. It just loops forever. It's just an infinite loop. The main purpose is to keep our program from running off and crashing, because the QEMU simulator may report some weird exceptions.

Once our program runs, it just keeps looping at this point, and never exits. Since it doesn't exit, naturally there won't be any problems. Think about it — isn't that the case? In fact, that is exactly it. In all the later examples,

we'll add this line at the end. It's really just to achieve that purpose. There isn't much more meaning to it. The real content comes before this loop. When we write our code, including later when you do the exercises, keep that in mind.

And then the last dot directive, which is also a direction, a directive, just tells us that this file is finished. It's that simple. This is one of our simplest assembly programs. Now let's compile it. When compiling, the command we run

is right here, which is make. When it runs, let me see, when it runs, it's through this gcc command, that it gets compiled. What exactly do these things at the end mean? We'll talk about that when we get to designing it later. So what does OBJ do?

We covered OBJ copy earlier. It's actually this: because after we compile this instruction, after this GCC command finishes compiling, it actually produces a -O, it produces an ERF file. This file is actually a standard ERF format file.

It contains many sections that may be used for debugging, or for other purposes. Because in our compilation options, we added -G, so the executable file that gets generated actually contains a lot of other sections that aren't really used at runtime.

Now, the objcopy command, can actually take these unused section contents Strip it away. In the end, it generates a single In the end, it generates a file with the same name, and that file Inside, it contains It only contains the executable machine instructions.

So that's essentially the effect we achieve. Let's run it. I've been talking for quite a while, and it might all feel pretty abstract to you. Let's take a look at the actual result. This is it. Down here are those two files, right?

As for this test.c.s, it's exactly what we just talked about. Let's compile it. When you do the exercises later, the environment is already set up for you. Don't type out complicated commands yourselves; just type make, and that's it.

As long as there are no errors, which means that program has been compiled After it's compiled, take a look — there are actually two more files in there. One is called test.bin, and one is called test.erf. This corresponds to our...

it corresponds to what we just had here — these two files, the erf and the bin. This is variable substitution. It's actually just the string "test". Now let's take a look at these two things. How big is this erf? 5316. How big is the bin?

Only 16 bytes. Why is it 16 bytes? Think about it, everyone. I just mentioned it — this This objcopy, what effect does it achieve? It strips out the extra sections from the ELF file, keeping only the final executable instructions.

See what I mean? So how many instructions do we actually end up with? You can count them. Now here's the code for you. Take a look — how many instructions are there? This generates one instruction. Even though it's a pseudo-instruction,

it actually corresponds to just one — two. Not this one. It's that one. What we're looking at right now is the add. The add is this one. One. Two of them Three of them And there's one more, right? It's this one Isn't it? So the actual instructions

are just four machine instructions As we just mentioned, one machine instruction is on RISC-V 32, one instruction is how many bytes? Four bytes Four times four is sixteen, so this last part, after it's loaded, is actually just 16 bytes

This program is just 16 bytes Now, if you want to look at this content, You can also take a look. We can actually run that makehex. You can take a look at this. As for makehex, go back and look at that one yourselves. It actually just calls a

hexdump program to print out that binary data. So these are our instructions. Now, if, suppose I gave you this instruction, could you reverse-assemble it and figure out what it's doing? I'll leave this for you to try at home. Actually, I've already taught you how to do it.

Among them, we know, granted, there are some instructions you haven't learned yet, but there's one instruction you definitely should be able to do, right? That's the add instruction. Where is the add instruction? As we mentioned earlier,

it's aligned on four-byte boundaries. So basically, here we have this is one instruction, let me mark it with a red pen, this is one instruction, this is one instruction, this is one instruction, this is one instruction. Where is our add instruction?

It should be right here, because if you go look at the source code, this is the third instruction, right? Now, this instruction, why is it B3027300? Well, as we just discussed, suppose I give you this data, can you figure out...

you do the return byte why does it correspond to what is the add x what x what, I forget you can return it as bytes later now, when you return the byte everyone should pay attention to the endianness issue in here that is, what you see is this

this is a byte so if it's a word think about it, what kind of number do these bytes actually represent it's actually 007302B3 and what I mean is it actually represents 0X 00, read backwards just read it backwards and why is the high-order part 0

So, like we covered last time, your function7 is just 7 zeros, remember that? You can probably guess this one too, it's just like that. And which of these correspond to which counters, you can go back and check that yourself. Work it out on your own.

I won't do that here. I think everyone, based on what we covered earlier, should be able to reverse-engineer it yourself. Of course, there's no need. Normally, if you have the source code, we can just look at it directly. We can use the code command,

and this disassembly is actually why we compile with -g, because we include debugging information. make code will actually use the objdump command we talked about earlier. and make a back edge for it. If you're interested, you can go look at the Makefile yourselves.

So this command... that's the corresponding command. This command... you see what it wrote here is 0073. It's already converted over. Did everyone see that? This tool has already done the corresponding conversion for us. Now, as for exactly why it is...

why it's x5, x6... actually, these 5, 6, 7... these three numbers... how they get extracted out of here... you can go back and work it out yourselves. I won't go into it here. This program does static analysis. We'll just... so let's run it.

Let's see how it gets to... the effect of the addition we just added, our code just now was looking at 1 plus 2. So we know that during the run, we should first see register x6 being assigned the value 1, and then, second step,

the value of register x7 should become 2. Right? And then, what should happen as the third step? We said the value of register x5 should become what? It should become 3. That verifies the result of our program run. Now, when we run it,

we just do make run, and that's it. As for what actually runs behind the scenes, we've already explained that to you. Oh, sorry, not run — it should be debug. If, while it's running, if you run into that QEMU, and it's not behaving properly,

and you can't exit, how did I force that QEMU to stop just now? Just press the Ctrl key, hold the Ctrl key down, and at the same time press A and X one after another. That will force QEMU to stop. You can go back and try it yourselves.

That's how you stop the emulator. Now, back to the main topic. Let's run the debug. Let's see the effect of Make Debug. As you can see, our program has already started running. Now, this thing at the front is something I typed in.

That is, after you enter this GDB, if you want to exit, how do you exit? And also, you can just press one First, press Ctrl C. For example, I press Ctrl C here. After pressing Ctrl C, type a Quit command. And then type it asks you Yes or No.

You say Yes to exit, and then it will exit fairly normally. Because sometimes some of you may find that you, using some less graceful, rather forceful methods, after killing this program, it may cause that QML, since we are running it in the background,

that process may not get killed. If that background service can't be killed, then next time you run it, it may fail. I hope you all understand what I mean by this. So when you come out later on, just come out using this approach of ours,

and don't jump out carelessly. Let's do a quick demonstration of this. Then we'll take a break. Let's demonstrate it. Still make a debug. Now you can see the program has started running. This 'read symbols from' thing, because our test.ef was cross-compiled with -g,

it comes with debug information. And as we discussed earlier, our config file sets the breakpoint right at the start position from the beginning. So as soon as it starts up, it actually stops at a special Linux 8000. Why is this value Linux 8000?

We'll cover that later. When we get to that part. We'll get to that later For now, let's not worry about it Because what we're focused on right now is just this assembly syntax So you can see, at the very beginning, our three registers

5, 6, 7 have values 0, 0, 0 respectively This is what we just set up in that config file to make it so that with every step we execute, we print these values to make it easier for us to see Now, this breakpoint here, is at the 'start' position,

and that actually corresponds to line 12 of our code, which is really just the first instruction. The goal of the first instruction, is to be able to take 1 and load it into x6. So right now the value of x6 is still 0 So it hasn't executed yet

Because that command hasn't been executed yet Now we want to step this instruction How do you step in GDB? Well, the command is called si si stands for Single Instruction Remember this, and these commands become easy to remember

Otherwise you just can't memorize them But if you remember the English, you understand what it means What does single instruction mean? It runs at the level of a single instruction Not a single high-level language statement We're talking about assembly instructions here

instruction So after single instruction As you can see our x6 value has already changed to what Exactly because there's no highlight it might look a bit unfamiliar to you It was still 0 just now and now it has become 1 This is the effect our instruction just now achieved

Alright Just get used to reading it this way So this is si Now for the next step, let's change x7 to 2 Actually, you don't have to type si every time I mean, once you've typed that command, just press Enter and it will repeat the last command

So all you need to do is press Enter You can see this 7 has already become 2 Right? So what are we going to do next? The next step is to do the addition. The addition takes the value in x7 and the value in x6, adds them together and puts the result into x5.

Right now x5 contains a some other messy value. So let's execute it once more. You can see the value of x5 has already changed. It's now become 3. See that, right? Then what follows is starting to execute this infinite loop. With the infinite loop,

if you press enter again, it will keep executing on this same instruction forever, so our program won't exit. Otherwise it might run off somewhere. This instruction mainly serves that purpose. So that's a simple example like this.

Everyone, go take a break. Try running it yourselves. To quit, just use what I mentioned earlier. Ctrl C, then pick quit to exit. Let's continue. So what we just saw was the first add instruction. Now, through this add instruction,

simple as it is, I'd like you to get a feel for some basic concepts of assembly, and through the exercise just now, I also walked you through the operating environment. So, next, let's go over some related concepts. That is, as we know,

when doing addition, when we perform operations on numbers, these numbers, they definitely have So to be able to support these positive numbers, negative numbers, these concepts, right? So in our computer, the related concepts involve

involve some... Let's take a look. First, we need to understand the concepts of unsigned numbers and signed numbers. So, the concepts of unsigned numbers and signed numbers, I think everyone should be fairly familiar with them.

We have these unsigned and signed concepts in high-level languages too. And it's the same in computers. So if we have a 32-bit piece of data, if it's used to represent an unsigned number, let's say, for instance, to keep it simple,

an 8-bit, if an 8-bit piece of data can represent unsigned numbers, then the top bit is definitely not needed as a sign bit, so its representable range is larger. And for signed numbers, we generally use the highest bit, 0 meaning positive,

1 meaning negative. That's the basic idea behind it. Now, to be more specific, how exactly are signed numbers represented in a computer? This is also some very fundamental stuff, because you'll probably need to look at values in memory,

or specific values in registers. So when we're dealing with signed numbers, their representation uses two's complement in binary. Right? Now, I don't know if you still remember the concept of two's complement. That is, if I give you a decimal number,

and ask you to express it in two's complement form, how would you do it? I wonder if you still remember. Do you remember which course you learned this concept in? Have you learned this concept before? You've learned it, right?

As computer science majors, these most basic concepts, we still need to go over them here, and try them out once more. So when we talk about this, when we get to two's complement, we're actually, here, not going to dig into all these concepts.

What we at least need to master is, if you're given an integer, how do you convert it into two's complement? Well, it's very simple. We say that if this number, if it's a positive number, how is it represented in two's complement?

It's just the number itself, right? The highest bit is simply 0, and the rest is just whatever the value is. Now, what about negative numbers? It's a bit more complicated. We say a negative number has to be converted into a binary two's complement.

There's a little formula for this, what is it? You still remember, right? Take its absolute value, right? Then after converting it to binary, invert it, and then do what? Add 1. Still remember this concept, right? Let's use a simple example.

For example, take this binary number, let's say it's 4. So for 4, if it's a positive number, its representation in the computer is 100, right? Now, if I use an 8-bit To store this 4 in an 8-bit byte, for the positive number 4, you just need to pad five 0s in front,

this doesn't work for me, and that's it. Right? This is a positive number. Now, what do we do if it's a negative 4? How do we calculate negative 4? First we take its absolute value, which gives the original binary of 4, which is 100.

Then what do we do? Invert the bits. What does inverting the bits mean? It becomes how much? It becomes 011. Right? Then what do we do? After inverting, add 1. So if we add 1, as we know, the result is 001. And then, because we need to compare an 11-digit 8-bit number,

the leading bits are since it's a negative number, what do we do with the leading bits? Pad them all with 1s. Pad with eight 1s. Not just five 1s and that's it. Right? So in the end, it becomes this weird number, 1 2 3 4 5 6 7, plus one more.

This is how negative 4 is represented in a computer. Now the other way around, if I give you a two's complement number, can you work out the original number it represents? Do you remember how to do this? I think it's done by keeping this

For example, suppose I give you a number like this, and you need to figure out what it is and how to do it. We just keep the first 1 from the right that is not 0, and then flip everything before it. Isn't that 4? Right? We won't go into this in more detail.

Because this thing, you can just look it up yourself. It should all be in there. So we won't say more about it here. But we do need to understand these concepts. So I'm just mentioning it here. Now there is another important concept,

which is the concept of sign extension and zero extension. This probably wasn't covered much earlier on. But when it comes to assembly, we need to mention it. What we call sign extension, that is, just like the example before,

That is to say, if this 0011, which we just computed a moment ago, is 100, here it only takes up 3 bits, but if we want to, there are still 5 bits missing in front, and if they haven't been filled in, then, because it's a negative number,

we do sign extension. That is, we fill in 1s, depending on whether its sign bit is 1 or 0. If it's 1, we fill everything with 1s; if it's 0, we fill everything with 0s. This is called sign extension. And if it's zero extension,

what does that mean? It means that regardless of whether this number starts with a 0 or a 1 in front, everything on the left is extended with 0s. That's actually what it means. These are called zero extension and sign extension.

And zero extension, sometimes, we also call it unsigned extension. So these are the concepts of sign extension and zero extension. We'll come back to these later, so I'm just mentioning them here. Now in English, sign extension is called sign extension,

and zero extension is called zero extension. Now this example, as for this example, this example, everyone, let me see, in today's class, we might be running a bit short on time, so we won't have you do it right here in class. This add2, actually this example,

let's take a look at what the code does. It's really just to let you see how a negative number is represented in memory and in registers. It's very similar to the example from just now. It's just that here we put a minus 2 in x7.

So in this x7 register, what exactly is the value in it? You can go and check. Then after the calculation, since we know that 1 plus minus 2 gives minus 1 in the end. So what does minus 1 look like when stored in x5? Minus 1 is a bit special.

From now on, whenever you see all f's, that's -1. Right? You need to get used to this. I won't have you do it right here on the spot. When you do it yourselves, the main purpose is to let you review the concept of two's complement.

That's the second concept. Next, let's look at subtraction. Subtraction is also fairly simple. Subtraction is pretty simple. As for subtraction, this I won't say too much about it. For subtraction, it still uses its type is still R-type.

And then, the specific format is to subtract RS2 from RS1. Here we use a minus sign. RS1 minus RS2, and the resulting value is saved into RD. That's subtraction. Now, as for the specific example, here's an example using sub. You can also go back

and practice it yourself outside of class. These are all provided in complete form. Everyone, don't just use them as exercises and sit there.
