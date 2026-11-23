# English Narration Script

Chapter 4, embedded development. Because when we go on to develop an operating system, honestly, it involves a lot of techniques related to embedded development. So here, as one chapter, I'll cover some preliminary content for you: I'll introduce the concept of embedded development, the concept of cross-compilation, and some tools and software commonly used in embedded development, including GDB, the QEMU emulator, and MIC.

So first, let's take a look at what embedded development is. So, embedded development, you can look at this diagram. This diagram is actually a if later on you go out and do this kind of say, developing drivers, developing operating systems,

that kind of work, then this is actually the kind of work environment you'll have. It's a bit different from our non-embedded development, that is, the common local development environment. It's a little different, because if it's local development,

basically just having one computer like this is enough. Right? For example, you just grab a computer here, and as long as it can connect to the internet, Basically, your environment is pretty much all set up. But if you're doing embedded development,

that is far from enough. In particular, we... because the programs we need to run often have to run on a separate what we call a target board. That is, on say, a development board, or on another machine. In that case, we at least need to set up a small development environment,

maybe through a router, connecting those two machines together. Now, this machine here, is called the target board, and this machine here is called the host, meaning the main machine, and this one is called the target machine.

on the host machine, generally, we do things like compiling, editing, and debugging. And this target board, what mainly runs on it might be running on another machine, right? Just get familiar with this setup. Now, embedded development,

it happens in a specific hardware environment, developing for a particular piece of hardware. So the connection with the hardware is quite tight. Very often you need to keep in mind that your program is not running locally, but on some special hardware.

What exactly is on that hardware, you need to be quite familiar with it, and only then can you do the development. Well, with an operating system, that's even more true. Because when you develop an operating system, you definitely need to know

the machine your operating system runs on is for example, whether the CPU is x86, or RISC-V, right? So at the very least, that's what you need to know. And you can't really develop it locally, because with local development, there's already an operating system on your local machine.

So of course there's no need for you to develop another operating system. Your operating system is definitely going to run on a machine that has no system on it, right? So, if you want to build an operating system, it actually falls under a kind of lightweight agile development,

and so So, since this is embedded development, meaning the target machine and the host are two separate machines, we came up with a concept, called the concept of cross-compilation. Now, what exactly is this concept? If you haven't done embedded development before,

you probably know very little about it. So I'll go ahead and explain it to everyone here. First, let's say we can define a few concepts: the build system, the host system, and the target system. I've already circled the important parts here

in black text, so feel free to follow along with me. What is the build system? The build system. What is it used for? It's used to generate the compiler and other auxiliary programs that let you control compilation. That is to say, the GCC you normally use.

This kind of compiler actually has to be built as well think about it, right? It's not just there by nature It has to be built too So how do we build it? On which machine do we build it? That machine is called the build system And once it's built, what do we do with it?

We run it, right? Exactly So on which machine do we run this compiler? The system where this compiler runs is called the host system So what is the compiler for? The compiler is used to generate another program think about it, right?

It is itself a program and it's used to generate another program And the application program it generates Where does it run? It runs on the target machine. So the machine that runs this program is called the target system. So we have three concepts:

the build system, the host system, and the target system. With these three concepts, we can define what native compilation is, and what cross-compilation is. The formula is very simple. You should be able to read it yourselves.

What does build equals host equals target mean? Let me translate it into plain language. What does it mean? It means that, for example, on this computer of mine, I built my compiler on this computer, right? And at the same time,

the compiler itself also runs on this computer, and the programs it produces also run on this computer. In that case, this is called native compilation. I'm also running my compiler on my computer, and at the same time, what I generate —

the application programs I build with my compiler — also run on the same computer I'm on. So what do we call this way of working? It's called native compilation. If you're not involved in embedded development, most of the work you do is actually native compilation.

It's just that you probably don't build the compiler yourselves — you probably just grab a compiler, right? But your computer — say your computer is x86 — then that compiler you grabbed, it was probably also built on a machine that —

most likely, it was also compiled on an X86 machine. So your build system and your host system, they're the same system. And at the same time, The executable program you generate with this compiler, doesn't it also run on x86?

So naturally, this way of working, is what we call native compilation. As for cross-compilation, in general, the way we define cross-compilation, is that the first two are the same, but mainly, the executable you generate, does not run on your host machine.

Just like in that diagram earlier, you generate an executable here, let's say it's called A.out, but this A.out, does it run on this computer? No, it doesn't. You have to use a network or some other method, to take this A.out application and do what with it?

You have to download it onto this board to run it. You also understand what I'm saying the processor on this board and the processor in your computer might not even be the same instruction set at all Your machine might be X86

while this one on top might be RISC 5 In that kind of situation, you get that cross compilation working environment Everyone can follow, right? We also have a diagram here that everyone can take a look at Let's take another look

This diagram is actually also talking about this same thing That is, combining the formula we just talked about If, for example, look at this This tool of ours is called... We normally type GCC But actually, when we type GCC this GCC is really just a soft link

I don't know if any of you have ever looked into it So where is this GCC of yours? For example, when we type gcc, what exactly is this gcc? We can type "whereis gcc", and as you can see, after we run whereis gcc, it tells us that your gcc is actually at this full path right here.

Right? So what exactly is this thing? Let's dig a little deeper. You can see that the gcc you're running is actually this thing. Now, is this thing actually an application program? Not quite. What is it actually? It's a symbolic link.

Do you all see that? The concept of a symbolic link. If you're not familiar with it, Read this one yourself. So this it points to that again. Now what is this thing again? Let's keep tracing. This one is still a symbolic link. Layer after layer of wrapping.

It actually wraps it up. This is for everyone's convenience. It actually points to this. Now this thing, if we trace it further, I didn't type that wrong, did I? OK At this point we've already... ZR8 is no longer a symbolic link.

This is the thing we actually run. In other words, when you type, when you type GCC, what you're actually running is this thing. This actually tells us it's actually a build tool that runs on an x86 64-bit machine a compiler So that means,

your compiler actually runs on x86 At the same time, we but in order to in order to compile our program, what do we need to do first? We need to build a compiler ourselves, because with this program, when we normally type GCC, the a.out you compile

can only run on the x86 machine You can't run it on a RISC-V machine You all know that, right? So if we just use GCC directly, it won't work. We have to build a compiler ourselves. Of course, the process of building that compiler

is something I won't go into here. This compiler, we've already built it for you, and placed it up front, in that tools directory that I introduced to you earlier. So this tool is made using that compiler, because once we have the source code,

we can use that compiler to produce a compiler for RISC-V 64-bit machines. Then when we do our daily work, we use this compiler for development. So we generate a newlib, and this newlib Because we're using a 64-bit machine and compiling this with the RISC-V 64 toolchain, cross-compiling it

so this program can it run on your local x86 machine? No, it can't. You can try running it and it will throw an error. So we have to take this program and put it on a RISC-V 64 target machine to run it. Now, this machine, when we do the experiments,

is actually emulated with an emulator. Of course, you can also actually go out and get a real RISC-V board. You can buy those on the market now. So through this process, everyone should get a feel for it. This is what we call cross-compilation.

Cross-compilation. not done entirely on the local machine it involves two machines the host and the target Now, for these kinds of tools, we also have some naming conventions, but I won't go into detail about them. From the earlier examples,

you can get a feel for what those names represent, right? As for the rest, these names, I won't elaborate on them. So, these, so the GCC we run, is actually the GCC we normally run, is actually a wrapper, it's actually this thing, it's this thing.

Its full name is this thing. If you're interested, you can look it up yourselves after class. That's the concept of cross-compilation. Alright then, next let's talk about debuggers. Because we often — especially in assembly programming —

when it comes to debugging, later on we'll when we talk about assembly programming, we'll be using a debugger. So let's also get to know debuggers a bit. Now, in the GNU development environment, there is a GNU debugger called

GDB. Actually, its full name is GNU's debug — that is, an abbreviation of GNU's debug. Now, as for this debugger, I wonder if any of you have used it before. Have you used it? Have you used it in your regular development? A debugger.

If you all use graphical interfaces, what you see may just be a graphical interface. But behind it, it's really this thing. It's just that many graphical interfaces, well, on the front end, they just wrap it with a GUI. So this GDB,

well, it's exactly this kind of thing. So when debugging, you can, that is, when we debug normally, it's actually a process like this. Debugging is divided into two types. One is called local debugging, and the other is called remote debugging.

So for local debugging, it's similar to local development. That is, the program you're running It runs locally. And that remote debugging, is really used in those cross-environment scenarios. Let's talk about local debugging first.

Local debugging is probably more familiar to you. It's very simple. After you generate an executable program, what do you just type? For example, you generate an a.out, and then how do you debug it? You just use GDB, right? Just follow it with a.out, and that's it.

Isn't that right? So what actually happens behind the scenes? In fact, when you run this, say you typed GDB, followed by a.out, and then hit Enter — what happens? Have you ever thought about that? Well, this may involve some some stuff under Linux

that is, it will once this GDB is up and running, it will first fork out a child process. Inside this child process, it will go and run run the program you're debugging, the target program being debugged. It runs in a separate process,

and then this GDB program of yours, also runs in its own independent process. Then the two processes, through the special ptrace system call, establish a connection. It actually makes that program being debugged, that process,

you set breakpoints, right? Remember? When debugging, you have to set breakpoints. The breakpoint you set Once it runs to this point, it will actually suspend this process, putting it into what's called a suspended state. You've probably learned about this before,

if you've studied process debugging, that state, the several states of a process. Can you still recall, how many states are there? Running state, what else? From running state, there's something called sleeping state, right? And then some kind of stopped state,

and finally it has to enter some kind of zombie state, right? You still remember? These concepts actually all come into play here. If you want to go look It will suspend this process and then we can look at the process some context information when it is stopped at this point,

like the information of all the registers, becomes visible. So this is actually called local debugging. Local debugging is this kind of process. Now, if you want to debug a program that is not local, like when we debug an operating system,

or when we debug some cross environments, then it actually needs to start a remote service called this GDB Server on the target machine. So actually my GDB application interacts with this service, and this service proxies our debugging process.

And then the program being debugged is running on another target machine, while your debugging machine is the local machine. The two machines have to be connected through the network. So this is called remote debugging. So later, when we give you exercises,

for example, when we do our demonstrations, it's actually a form of remote debugging. And for us, this target machine, we also use an emulator to simulate it. We use an emulator to stand in for a real one. As for GDB, it supports various languages.

We won't go into that. Now, as we mentioned earlier, we will use an emulator to simulate the target machine many times later on. Of course, we could also get a real machine, but that's too much hassle. So what is an emulator?

I won't go into the GDB debugging workflow here. It's very simple. You all surely know the basic workflow. So let's take a look at the emulator. The emulator we'll mainly use later is called QML. I don't know if you've heard of this one.

It's used quite a lot nowadays. QMU actually stands for Quick Emulator. Emulator means simulation, right? So QMU is Quick — basically a fast emulator. It was written by a French engineer named Fabrice Ballard. What is written You could say this guy is

fairly well known in the computer world. He's a bit of a prodigy. He's developed a lot of interesting things, and QMU is one of them. It's quite popular now, and it's often used for system emulation. Now, this software mainly runs on things like Linux.

On this kind of system, it can support multiple architectures, emulating a real machine. Now, as for this software, QEMU, it has two main modes. One is called user mode, and the other is called system mode. Just so you all get the idea,

user mode means it runs an application directly. What does that mean? Well, for example, suppose I've just run and produced an A.out executable program. Let me give you a quick demo. Let me see if I have... if we have time, we still have a little time,

so let me give you a quick demo of the basic use of this emulator. That is to say, after you go back, you can all try it out yourselves. Actually, this example I gave you, it corresponds to the one in our exercise, this one in the exercise,

4.4-2, 4-1 we actually kind of did along the way as well. 4-2, that is to say, now, first we need to cross-compile, we're not doing native compilation here. So suppose we still have that application program from before, suppose we still have this program,

but for this kind of program, now we want to turn it into a runnable executable, It's not running on x86. We're running it on the RISC-V platform. Of course, we run it using an emulator. So how do we do this? Well, actually, first of all, we know we need to compile it.

Now, for compilation, can we still use GCC at this point? As we discussed earlier, you can't use GCC. We need to use a cross-compilation environment. And we've already provided that environment for you up here. So it's called RISC-V,

a GCC that starts with that prefix. The full name is this. So we use this GCC, and first generate the build. Let's take a look. Now, when compiling, you need to add some parameters. for example, it's called you can probably still read this

RV32 RMA if you listened carefully in the last lecture you should know what this means what does it mean? it means the program we want to generate is a RISC-V 32-bit system supporting which instruction sets does this CPU support?

the integer instruction set M is multiply that's the multiply and divide instruction set and what about A? that's the atomic operation instruction set you understand, right? it's these things that is to say, the program we compile and generate

can run on a CPU that supports these instruction sets And then we specify it again not this one I'll just put it right here I won't explain too much for now it's mainly specified through this for example, how many bits an integer is

how many bits a long integer is I won't explain this part and then after that, we can just directly the syntax afterward is the same why is it running so slowly okay that is to say the main difference from our usual GCC is actually just that this part is different

we need to use a cross- compiler for RISC-V to do the compilation so this way And it generated an a.out file. This a.out, everyone, pay attention here. This a.out, well, it's actually not for x86. You can use the file command to take a look.

It will tell you what kind of machine this is. It's a RISC-V one. If you try to run this a.out directly on our machine, this a.out, it will throw an error. Sorry, I mistyped. See that? It says it cannot be executed. Why? Of course it can't be executed,

because inside it, these are RISC-V instructions right? So how do we execute them? One approach is to take this A.out and put it on say you have a RISC-V board you just take this adenaut via a USB drive or whatever method or the network, and burn it onto the board

then just run it on the board Of course, we don't have that board right now so we'll use an emulator to run it That's the idea of an emulator So how do we run it? We run our QML application As I just said what we're demoing now is called user mode

which means we just run adenaut directly on top of it so it's basically like Your program, it just runs as if it were running on a Linux system, and it can run this program, and then print out hello world. You all understand what I mean?

So this is one way, it actually just simulates being able to execute a process, open a process, and just run your program. So that's one way of working. And then there's another way, which is when we develop this operating system later on,

because as we know, when developing an operating system, the thing is, you can, how should I put it, That is to say, when I run it this way, you can think of this this emulator as actually simulating down to which layer does it simulate?

Let's combine this with the diagram from earlier, let's pull up that diagram and take another look. Think about it. That is, if when I use this approach to run an application, this emulator, which level does it simulate down to?

which level does it simulate down to? which level does it simulate down to? It simulates this level here. Think about it, isn't that right? Because this application Isn't it an a.out? Think about it, isn't it? Isn't it? It's actually simulating at this level.

That's why it's called the user layer. User mode. Because this program... This program is a program running in user mode. a.out. But later on, when we develop an operating system, can you still run it this way? That definitely won't work.

Because in this mode, our simulator actually has to simulate at this layer. Think about it, doesn't it? It has to sink down one layer. Go down one layer. And a simulator of this kind is what's called system-level emulation. So this QEMU of ours

has two main modes of operation. This user mode is the one I just demonstrated for you. Now, if you want to use an emulator to develop an operating system, then what you need to simulate is a system-level layer. You have to simulate the entire computer system,

essentially as a whole starting the simulation from the logical level. This approach is called system mode. Our later development will mainly focus on system mode. We'll demonstrate that to you when we get there. Here, we're mainly using user mode

to give you a sense of how this QEMU is used. It can emulate a different architecture. I won't go into that now. Later, when we get to system mode, we'll take a look then. So as for emulators, that's all for now. Everything has been covered fairly quickly.

Later on, when you go through the exercises, we can take our time and get a feel for it. So, the last one, is the make tool. I think most of you have probably used this tool before. So I'll just talk about it briefly myself. That is,

we know what the make tool is mainly used for. So, what's your own understanding of it now? You must have used it, right? You must have used it. Right? That is, when your project gets big, if you only have one file, you don't really need make.

Right? But if you have quite a few programs, quite a few files, then you need an automated way to help you compile and build this system, you'll use make. So, make, it's an automated management tool. Specifically, it only knows how to... that is to say,

how do you tell it to build the system? Actually, it's through a makefile. You can think of the makefile as basically we write some statements in the makefile to tell make how to carry out this automated build process. Right? So how does make

find this makefile? Very simple. By default, it will look in the current working directory, to look for that file with the same name Now, if you want to specify a particular makefile, you just add the -f parameter. I won't go into that.

So the most essential point about a Makefile, the syntax inside it, is actually made up of one rule after another, right? You all still remember this format, right? Now, the thing to pay special attention to is that each rule,

consists of several parts. The first part, is a target, followed by a dependency. And then, what comes below, are commands, one after another, right? The part in front... Over here, this must be a tab. When you write this, make sure you never

get it wrong. There are also some explicit syntax rules. What do I mean by explicit syntax rules? Let me mention that briefly. A single file can contain multiple rules. Right? You might write several rules, quite a few rules. So one MAC file might have several rules in it.

Then when I run MAC, how does it know which rule to execute? Which rule does it execute first? If you don't specify one, how does it behave by default? As you probably know, if you don't explicitly specify it, by default it takes the first rule

as its default rule and runs that. Right? If you want to write it further down, you need to add one at the front, .PHONY or .DEFAULT_GOAL, which is the default rule, the default target, to specify it, to make it clear that this is the default one,

and that's it. If you use this method, it's fine to put your default rule at the end, it doesn't matter, it doesn't have to be the first one. Now, there are also some phony rules. Phony rules, in fact, mean they specify certain actions,

but they don't participate in the rule derivation. Right? Let me just quickly walk you through these. And then this part after it, I think I'll skip those that is, the derivation process of this rule I won't go over that with you either

So over here, I still have a bit of time so I'll use it to, that is, tie it right into this makefile and walk you through it That is, some of the makefile stuff in our code, it's a good chance to get everyone familiar with it again.

So, okay, let's take a look here. Of course, if you're up to it, you should be able to read it on your own after you go back. I'm just walking through it with you here. The makefile in this project of ours. Ah. Once you've finished downloading this directory,

open it up. Uh. All of our code is placed under this directory. Uh, we covered this above. This is ASM, some assembly code. And the operating system is this code. Now, if we take one of the examples, we know that in here, there's a makefile, right?

Let's take a quick look at this makefile. As for this makefile, it's a good chance to review things with everyone. For instance, this one here, what is this called? What is this doing? This is defining a variable in the makefile, right?

This is a variable name. The equals sign after it. After that is really just a string substitution. Right? We won't go into the specifics. Here on the left, it's defining some variables. And then, this is also defining a variable.

Now this one, can everyone still follow what this is doing? This is a special syntax in makefile. What can it do? It can take the contents of this variable and substitute them according to a pattern, replacing everything with the .s suffix

with .o. So what is this thing? This thing, we just expand this thing. So after this line is executed, what does this objs variable actually become? It just becomes start.o, right? Everyone gets that, right? Let me walk you through it again.

Since we're not doing a dedicated makefile course, So, to avoid people not understanding this file, I'll go ahead and explain it. And then, what does this plus-equals mean? It means append, right? Append. So this likewise takes what's in this variable

and does pattern substitution. Then after this substitution, this becomes kernel.o, right? So what does this obj end up becoming? Doesn't it become start.o space kernel.o, right? So these two .o files are exactly the two object files we're ultimately trying to generate.

That's really all there is to it, right? OK. Now, look at this os.elf — what is it? This is our real goal. The thing we want to end up building is this target. What does this target actually depend on? It's os.elf. This is the kernel file we want to produce.

Does everyone follow? So we keep recursively tracing this file's dependency chain layer by layer. So what does the elf file depend on? It depends on OBJS. This OBJS, as we said, is what we just computed over here. It's that kernel.o and that start.o, right?

What does that tell us? It tells us that for our kernel's executable program to be built, those two .o files have to be built first. I want everyone to understand what I mean here. So how are those two .o files built? It's accomplished through this command down here.

Okay. So basically, this rule is derived this way. And then down here, it's also this kind of syntax in the makefile. It actually tells us explicitly when we want to build a .o file, what kind of command should be used to do it.

Now, this cc here, actually just means using that compiler. Alright. The rest are just phony rules. These, for example, when you want to run it, you just type make run. If you want to debug, you type make debug. Right? This make debug

you can see what it runs when debugging, it runs QEMU and the run target also runs QEMU which is the emulator. So it starts up that emulator, and then takes the kernel image we built and loads it into the emulator to run. This is the concept of cross development.

Even though we're using an emulator, it's really the same idea as debugging on a real board. Alright, basically the Makefile, I'll just briefly cover it up to here. After all, we're not specifically here to teach the Makefile.

If any of you have any questions about the Makefile, since it can actually get quite complicated to use, if you have any questions You might need to go take a look at it yourself. Or if there's a chance, maybe we'll do a Makefile course later.

Because that's not the focus of these courses. So we just skim over it, giving you a quick pass. So the main point is, I hope everyone gets a sense of one thing: through studying these courses, the idea is really, what should we let you understand?

That is, to develop a kernel, what knowledge do we need to master? See what I mean? This is basically the idea of a knowledge map. That is, through studying these courses, we need to... if in the future you want to go into work in this area,

which pieces of knowledge do you need to master one by one, and then stack them together? Then finally you can go develop a kernel. So these related dependencies, we'll just cover them in actually, the preliminary part. Basically, next class

we'll start talking about assembly. This is also a key part of our course, because for low-level development, you can't get away from some assembly language stuff. So at least before we talk about assembly, all of our preliminary material

is pretty much all we have. The related techniques, how to use these tools, you may need to take your time later on and gradually get a feel for them yourselves. Okay, today's class will end here. Alright. Thank you, everyone.
