# English Narration Script

Let's get started right away. Now we enter Chapter 1 of developing a small operating system. Of course, the whole course is Chapter 7, called HelloRVOS. As I mentioned earlier, RVOS is the name of our system, and we'll use it to refer to it from now on.

So for this part of the course, in this chapter, we mainly deal with system boot, and we need to, for this operating system of ours, for our operating system, we need to be able to see a hello world, of course, what we print is hello rbos,

that is, the first step is to get it printed out. And of course, we print it out through the serial port. We can see a most basic display process. Now, these are our reference materials. 1 and 2, these are all basic instructions.

As for the third part, it mainly covers some serial port content. It's a technical manual for our serial port. We'll come back and look at it later when we get to that. Let's first look at the first part, which is system boot.

As we said, for an operating system to run, So the first part is how to boot it up how to boot up the hardware. Now, since we need to talk about these boot concepts, when it comes to the hardware, we need to have some understanding of it.

Because if you don't understand the hardware, you won't be able to develop low-level software. So let's first take a look at some basic concepts of the hardware. Now, the concept of hart, we've already mentioned to you before.

As you know now, a hart is actually, you can think of it as a processor. Inside this hart there are some registers and so on. Of course, my diagram here isn't very accurate, but the idea is basically this: the hart we're talking about now,

in the RISC-V field that we're in, it's essentially the concept of a processor. So, when we talk about the board we're working with, this emulator, inside it, it's actually made up of multiple harts. It supports multiple harts.

Now, besides the processor, there are also things like memory, and IO, which stands for input/output, and various devices, and so on. Down at the bottom, you can think of these as peripherals. Now, these peripherals, as we know, on a computer,

are connected to the processor through a bus, right? So let's separate them out, to make things clearer. So, in the end, we take the processor cores plus some external devices and then package it all up so what's exposed to the outside

may just look like a single chip So, on this chip, in this embedded development of ours, we often use a term called platform. Of course, if we translate it directly, it's called 'platform'. But in reality, it represents a kind of chip,

the concept of a chip, or you could say a... Why is it called a platform? Well, we can actually start from this, from the development process of our processors, and I'll briefly explain it to you. That is, if we look at the early days,

because there wasn't that kind of, say, things like mobile phones or anything Not that many. The one we come into contact with most is the PC. And a PC works the same way. That is, it has one processor, right? And then, plus a lot of peripherals.

So this, we call it a motherboard. So this thing can be seen as a kind of platform concept. Now, this thing, we say that with the development of computers today, these peripheral devices, they are getting more and more integrated.

More and more integrated. In the end, you'll see that in some of the typical small boards today, these highly integrated boards, there are fewer and fewer peripheral components on them. Of course, comparing the picture above with the one below,

see if you can spot the difference. In other words, in the picture below, apart from the big black chip in the middle, there are very few peripheral devices around it. Have you noticed that? So where did the peripherals go? Actually, the peripherals haven't disappeared.

They've just been integrated at a higher level onto this chip. Do you understand what I mean? Now, for this kind of chip, we now often, in the embedded industry, call it and you can see a term here, which is called SoC. It is actually

an abbreviation for System on Chip. So what is System on Chip? It means that an entire system has been put onto a single chip. Understand what I mean by this. Now, this thing, the SoC is what we call the platform. In practice, the term 'platform' usually refers to exactly this kind of thing.

It's a concept. Now, our current emulator, our QEMU virt, this 'V-I-R-T' is actually 'virtual', meaning the concept of virtual. So on this QEMU emulator, there's a simulated platform called 'virt'. Similarly, this thing also contains a lot of

it actually contains a lot of processor cores, as well as some peripherals, including memory. ROM is read-only memory, and DRAM is dynamic memory. ROM means that, after power-off, its contents are still retained. RAM, on the other hand, loses everything when the power goes off.

When we usually talk about memory, we mostly mean RAM. ROM, you can think of it as a small storage chip, something like a tiny hard drive. Now, this QEMU platform, this virt platform, has a total of 8 cores in it. A total of 8 cores.

So first, everyone should get a basic understanding of our hardware here. Now, when we go and operate on these components on top of it, pay attention to one concept, which is the concept of address mapping. What does that mean?

It means that in the design of our hardware, the currently popular kind of platform, it maps all of these peripherals What it operates on, the objects of those operations, we know that a peripheral, say a processor, or a controller,

may have some registers on it that need to be operated on. These registers have all been assigned, and when we access them, in the past, each register had its own separate address for access. Now, all of these peripherals have been given a unified addressing scheme.

These are physical addresses, not virtual addresses. A unified addressing. So we can see that, that is to say, this structure, actually comes from, I was from the QML... It was extracted from the original file, cut out from there,

and what it actually shows is this: all of its on the platform, the various peripherals involved, such as the ROM, right? And then, and then down here is the RAM. In fact, there is a... each peripheral, it corresponds to an address range.

Now, for the RAM, this address range is straightforward, meaning that its... for example, for this RAM, on the actual hardware, it should be 128 megabytes in size. a chunk of memory like this so this 128 megabytes of memory it is actually laid out in a unified way

in terms of how it's addressed meaning its starting address is 0x8000 and the end address is written like this even though it says 0 in reality there is a range it's 0x8000 plus 0x8000 plus 128 megabytes roughly this kind of range

so every component has a unified physical address mapping you can just picture it like this that is, all of our physical addresses from 0 to the highest address are allocated allocated away in chunks, for example, take this ROM chip:

it, the storage space inside it, it corresponds to the range from Linux100 to Linux, FF00 — that kind of range. This range is actually shown on this table here — it has this one range. And then, the RAM also has a range like that.

So 0x8800 up to that, if you work it out, this space is about 128 megabytes. If I remember correctly, you can go back and check, a 128 megabyte range — meaning it can only take a maximum of 128 megabytes of memory. So it's basically like this.

Then all the address lines are given a unified numbering And this kind of address, that's the concept of a physical address. The concept of a physical address is a fixed mapping So later, when we want to access these things, we do it all through these addresses

to access these components. This is something you all need to understand. So right now, we're mainly discussing the system boot process. Now, in the system boot process, what's mainly involved is the ROM component and the RAM component.

So, as for everything else, Let's set that aside for now So how does this boot process actually happen? You can think of it this way: when we power on when we power on, our CPU starts running, right? Right? Now, when our CPU starts running,

these are all things at the hardware level. We can't see them from software. On the hardware side, first it will jump to the address 0x00001000. We know this address is actually mapped to the ROM device, right? This ROM, we say it's really just a

you can think of it as a small hard drive. It stores a small piece of stuff inside. This stuff, after power is lost, it will not be lost. It's always there at that location. Every time you power on, it will go to that location and execute the instructions inside.

Does everyone understand? It's executing the instructions inside there. So the instructions in there, the stuff stored there, we call it a kind of bootloader, a bootstrapper, a boot loader. At this point, what's inside is not our kernel yet,

because our kernel is over here. So the instructions in this section are hardwired into our chip, they cannot be changed. Then it starts running, and while it's running, the work it needs to do next we can actually go and look at what's inside it

of course, since we're using a software simulation take a look at this if you're interested, you can go and take a look at this source file, boot.c it actually contains a piece of assembly instructions from the bootloader as for its main job

I won't explain it here if you're interested, you can take a look yourself the effect it actually achieves is that after it gets our program running it then jumps to the address 8000 to continue execution of course, if this were real hardware

the bootloader might do quite a bit more work but since what we have here is a software emulator it doesn't really do that much in there strictly speaking If it's hardware, you can imagine this bootloader actually has to do a few more things.

For example, it has to initialize the hardware, and at the same time it probably needs to move things from the RAM and copy the real code into this memory. Of course, right? Because this memory, inside this memory, in fact, there is nothing at the very beginning.

The real stuff, it might be sitting on a hard disk outside, or it might be on an SD card. So when this thing needs to be loaded in, who does this job? Of course, it has to be done by the bootloader and things like that. Right? But for our software simulator,

there is no such process. But you have to understand, the process is roughly like this. It will also jump to this place to execute. So we can see that when we run the emulator, combining that with our earlier use of QEMU, in our QEMU command,

why do we include a -kernel parameter, followed by OS.EF? This thing, OS.EF, is actually what we compiled, the code of this kernel. Now, the -kernel option, essentially tells our QEMU that for our kernel, we just need to load this thing.

Because when we actually run it, it will jump there to execute. So, at this point, So what this actually tells us is, once it jumps here to run, then our kernel instructions, which instructions are they? then QEMU will automatically

take this portion of instructions and load them into this address space, and then it can continue running from there. You see what I mean, everyone? But this is entirely a software simulation process. With actual hardware, of course it is the hardware that carries out

this loading work, this boot work. But in any case, our program here, as you can see, it is the address 0x800, and why is it so special? This address is where we everything we compile the address of the kernel's first instruction

must be this address the position of the first instruction must be right here because when it finally runs, it jumps here to start executing that's the idea so this is also why we why is it that when we link, all these concepts tie together

this is what we talked about earlier when we link, why do we need to including when we wrote that assembly earlier, why do we need to at compile time everyone go back and take another look why use -Ttext? it's really telling our linker

In the end, because ultimately the address of every instruction is determined by the linker the linker does this for us right? Of course, what we use right now is static linking. So with a static linker, when statically linking,

the linker has to decide the address of every instruction, and it always needs a place where the addresses start from. And this address, this is what we tell it through this command here. So why is it 0x80000000? Now you understand, right?

It's simply because our emulator will place it at this location. and execute it. So we need to make this address match up with that address. So this value isn't something you just write arbitrarily. Now, through this process, on the one hand, you've understood

the whole boot process of our emulator, and on the other hand, you've also learned why, when we compile and link, we have to specify this link address, and how our QEMU loads our kernel. That's basically the whole process. So the entire system boot process

is pretty much everything I wanted to cover for you. At the same time, I want to mention one more thing here, which is that since we have 8 cores, you all understand, right? We have 8 cores. Each of these cores, after power-on,

these 8 cores run independently. As we mentioned earlier, what is a hart? A hart means that it can fetch and execute independently. So each core will repeat this process, this boot process. So this boot process is actually 8 cores running at the same time.

As soon as power is on, they are all running simultaneously. This is a concept we need to understand. Once we understand these things, these are hardware concepts. Now what we mean is, how do we use software? By software, what we mean is,

the code that we've entered here. How should we write it? The code that we compile How should it be written to carry out the kernel initialization process? So this is our boot program, that is, the boot program of our kernel, and the question of how to write it.

Now, our basic idea is actually very simple. As soon as it powers on, for now, because of our course, just to simplify the difficulty of debugging and explaining, we actually only want to support a single core. That is to say, you can think of it this way,

in hardware, although all 8 cores power on at the same time, and run at the same time, we only let one core operate. You see what I mean? The other seven cores, we just let them how should I put it, keep them in a state where they're just idling,

not letting them run our program. That's all we need. This is one of our design principles. Of course, if you really want to get that core running, that's also fine. Right now, in order to simplify our later explanations and the learning of the material,

we'll keep this issue simple. Even though there are eight cores in the hardware, we'll only let one core run. That's the idea. Now, the basic setup is actually very simple. That is, as we just talked about, the meaning of the previous diagram is that

This boot process, all eight cores are running at the same time, so how do we make... think about it this way: our boot program, the program we're going to write, is executed once by every single core — every core runs through it once.

Do you understand what I mean? This logic, every core will execute it once. So what do we do? Well, we can do this: once this program starts running, we can check, say if there are eight cores, then we can check whether this piece,

whether the core currently running this is the first core. If it's the first core, we let it continue on and execute the rest. That is, it goes into our normal kernel processing logic, but if it finds that it is not the first hart,

then we just enter a spin loop. I don't know if everyone can still follow what I mean by this concept. For example, suppose there are eight students in your row, and you are the eight harts. Among you, we only let the first student

run our operating system code, and the other seven students just spin. So how do we do that? Moreover, for all eight of you students, as soon as each one powers on, it will execute the boot logic. Think about it. Suppose the one on the far left,

you are the first student. Then when you judge, when you execute the boot logic, you are able to read out What this currently executes is the code number of which core, you see? So the moment you look at it, I am the first core.

OK, then I won't spin idly. I'll go off and do the real work. So it goes into our per-core logic. Now, the second core, if it is also executing this piece of logic, look at my core — right now I am not the first core. The first core's code number, say, is 0,

and my code number is 1. So I am not equal to it. Not being equal, I go off and spin idly. Understand? So what this achieves is that the other seven cores just spin idle the whole time, and only one core runs. This is our current bootloader's

basic design idea. As you read the code, keep this concept in mind, and you'll be fine. Now the question becomes, how do I figure out whether the current core is the first one? And how to initialize, and how to jump into the C execution environment?

Because for the large chunks of code that follow, we definitely want to write them in C, not in assembly, since writing it in assembly is too complicated. Now, the next two questions are actually fairly easy to solve. Because the earlier material,

the relevant knowledge, has actually all been covered before. Right, everyone gets that? But the trickiest part, probably the first one, is how to determine whether the current hart is the first one — this concept of 'hart'. Let's focus on that question next.

That is, how to determine whether the current hart is the first one. Now this involves some concepts that we didn't cover, when we talked about the RISC-V architecture earlier, some concepts we haven't discussed. As we mentioned before,

besides the 32 general-purpose registers, that is, x0 through x31, for each privilege level, in fact, we actually have our own set of specific registers. Right now we have machine, and we have user, What supervisor These modes.

Each mode has its own set of registers. This set of registers we call them control registers. As for how we read your hart ID, this number, it is actually read from these registers. Does everyone understand? These are some special registers,

called CSRs, CSR for short. Now for these registers, we we have also written it here, that is, each level has its own set. And then, at a higher privilege level, it can access the lower level, but not the other way around. Right? That's easy to understand,

since it's about access control. In user mode, of course you cannot access registers of machine mode or supervisor mode, can you? Just like user mode cannot access kernel mode. So, how do we access these registers? Well, there are special instructions.

These are not standard integer operation instructions. They are a type of instruction called the CSR extension instructions. We said that RISC-V's instruction design is modular, right? So for these special requirements, It defines a set of special instructions

to carry out these operations. When we implement these operations later, we will mostly be dealing with these privileged instructions and privileged registers. So let's first look at Machine mode, because we know that as soon as the power comes on,

the CPU runs in Machine mode by default. It's a bit like the real mode in x86. So what we need to focus on is what happens right at power-on, since we are currently in that power-on state. At power-on, how do we operate in Machine mode

to perform the relevant operations? As for Machine mode, the registers are quite numerous. You can see that it's listed here in this specific you can go and refer to it Reference 2 is the privileged instructions chapter 2.4 of that specification

this table in chapter 2 I've listed it right here Now among them look here, there are a bunch of these Machine Information Registers that is to say, things about the CPU, like the Hart and some ID values can be read out from these registers

Let's get a basic understanding first we'll cover the details later So the key point is, how do we read these registers? As we just mentioned, These registers cannot be accessed using the ones we talked about earlier, the arithmetic instructions

and so on. We have a set of special instructions for accessing these special registers. So here, there are just a few of them, like CSR, RWCSRS. And correspondingly, there are also some that can be accessed using a bit-manipulation style.

We won't go into the details here. We mainly focus on the ones we care about. Two of them are especially relevant, namely these two. These two These two instructions Let's first take a look at the CSRW instruction This instruction

its full name is Atomic Read Write CSR So this RW you can actually understand it as the abbreviation for Read Write And CSR is simply CSR, Control and Status Register Now here, there is also an atomic operation Atomic refers to the concept of an atomic operation

Let's take a look together at what this means So this CSRW instruction how does it actually work? We have an explanation here Let's all take a look at it together That is to say, it first reads the value in the CSR. Because this instruction

its specification works like this: RD CSR RS1 it first takes out the value in here, and zero-extends it to the width of the XLEN bits. This is easy to explain. You've already seen this many times before. What does zero-extension mean?

It means the missing bits are filled with zeros. And then, where is it written to? It's written into RD. It's written into RD. Then the value in RS1 is written into the CSR. How many steps is this? This is two steps. That is, the first step is to

move the content over here first. Second step. Move the content over here again. Everyone gets it, right? It's very simple—just these two steps. But here's one thing to note. The concept of atomic. That is to say, these two steps must be atomic.

What does that mean? It means that these two steps, the so-called atomicity, means they are completed within a single instruction, and cannot be interrupted. This is what makes this instruction slightly special. It actually accomplishes the effect of two steps of

moving registers in one atomic operation. Let me give a simple example. For instance, here is an example, say there's an example right here. What does this mean? This mscratch, this is a code name for a CSR, so it's a kind of register we'll be using later on,

a CSR register. Now, based on the explanation we just gave, the effect it achieves is this: first take the value inside mscratch and put it into the t6 register, which is a general-purpose register. First do that, and then take the value of t6

and put it back into here. We can write the equivalent like this. Now think about it — what effect does this actually achieve? It swaps the values of the two registers. Do you all understand what I mean? But what we need to note is

this swap operation is completed within a single atomic process and cannot be interrupted Its only difference is right here Now, in the rd position we can also place x0 We talked about x0 earlier So if that is the case then in fact,

the first step we just mentioned becomes completely meaningless Sometimes we use this We will see it later and when we do, we will go over it again So this is our basic instruction that is, CSRR This is our basic instruction Now, in common situations,

here comes another instruction In the usual case, we use this instruction to implement a write operation to the CSRSR instruction called CSRSR. This is a pseudo-instruction, and it is actually equivalent to writing it this way.

Now, think about this form, here it writes to x0. Based on what we just explained, how is it actually implemented? It writes this value over here, that's the first step. Then it brings this value back over, that's the second step.

Right? Now, as we covered just now, if rd is x0, the first step is actually meaningless. You see what I mean? So this first step Actually, there won't be any any activity at all In fact, at the hardware level, it means that it simply doesn't consume power

Put simply, in this situation, it won't it won't perform any action at all It will only perform the second step, writing RS into this Now think about this effect: isn't it essentially equivalent to performing a write operation on this CSR, this special register

performing a write operation? Don't you think so? So this instruction is actually what we frequently use this command to write to a CSR register This is what I want to highlight for you as a key point Of course, this instruction

It also has other uses, and when we come to use it later, we'll take another look. So this is CSR RW, and then there is another instruction called CSRS. Now, this one is called Read and Set Bits in the CSR. So let's take a look at the idea behind it.

What it means is, it first reads out the value in the CSR, and after extending it, it writes it into rd. In other words, there's nothing special here; that's just how it works. The first step does exactly this. It's essentially just doing some zero-extension,

which is not our main focus here. And then, however, For the next step, it is different from the previous command. It checks the bits of RC one by one, and if a certain bit is 1, it sets the corresponding bit in RC to 1. That is to say,

it embodies this idea of setting a bit. What does that mean? You can think of it this way: this part here, it is essentially a, it has the effect of a mask. I don't know if you have encountered this kind of thing in C, the concept of a mask.

When we run commands, there is this kind of concept, that is, when you change the mode, like 777, 666, 664, you remember that mask concept, right? There's a command called MUSK, right? So it's basically like let me explain it here

Basically, this is doing a mask operation What does that mean? It means that if a certain bit in this is 1, then that bit will actually become 1, meaning a certain bit in this register becomes 1. If it's 0, that bit actually stays unchanged.

So that's the effect it has. Also, let me emphasize again: these two steps are still carried out atomically. Now, later on, in our exercises, we actually won't focus much on this setting of bits. When we use the CSRS command, what we mostly want

is to use this command Now, for CSR registers, to perform a read operation, let's take a look. We actually have this instruction called CSRR. The R stands for read, which is just the opposite of the write one we saw earlier. Now, as we explained earlier,

it is actually equivalent to taking the rs1 field, and replacing it with x0. So we know that, what effect does it actually achieve? As we explained earlier, the first step is to take the value in the register specified by this CSR instruction,

read it out, and put it into rd. But here, since it's x0, But for x0, its value is simply 0. Think about it — if we use 0 as the mask to AND it, will that actually affect the contents of the CSR? In fact, it will not cause any effect at all.

Right? So this second step, in effect, does nothing at all. What we care about is here, conversely, we care about its first step. Now, what this step actually achieves is that it takes the value in the CSR register, reads it out,

and puts it into rd. That's exactly the effect we get. So we often use this instruction to implement read operations on CSR instructions. Now that we understand these two instructions, So we can actually go ahead and do our thing

Because what we need to do next is to figure out whether the current hart is the first hart Now, how do you think we should do that? It basically comes down to, during execution we first take this there is a register a CSR register

we read it out and take a look right? We definitely need to read it Now, this register is what we mentioned earlier there is a special CSR register called the hart ID register Its code name is mhartid The m stands for Machine mode

This register is from Machine mode the hart ID So when we read this name If we split it into three parts, it becomes easier to understand. Now, as for this register, it is read-only. It holds the hart ID of the hart executing the current instruction.

Right? So once we read it, we can find out who we are. In other words, reading it tells us who we are. Do you all get what I mean? Now, the specification states that each hart ID must be unique, and there must be one hart with ID 0.

So in general, in our hardware implementation, the first hart ID is 0. There isn't much ambiguity about this. Good. So what we need to do now is read it out, and then check it. Right? So how do we implement this? Of course, at this point, since

we haven't entered the C language programming environment yet, we can only write it in assembly. As we learned earlier, the assembly syntax is very simple. The logic in the previous part— when our program starts running after power-on and

say, it jumps to our kernel, at that NX8000 location, it starts executing the first instruction. And that instruction is this CSR instruction. We talked earlier about what this instruction does. It takes the value in that register

and reads it out into t0. Right? After reading it out, our second statement here, actually isn't really related to this logic. Let's not go too fast for now. So next up is BNE. What does this mean? We covered this earlier. What is this doing?

This is a branch instruction. What is it doing? It is checking the value in t0. If it's not equal to 0, it branches. That's what it means, right? Not equal. Branch if not equal to 0. That is, if it's not equal to 0, branch. Now for the first CPU, its value is 0.

It is 0. So it does not branch. If it's anything else, it naturally branches to the PAC label. Isn't that right? Now as for the label, What we wrote here, what it actually jumps to is exactly this kind of logic. This piece of logic is just an infinite loop.

That is to say, if I am not the first hart, then I'm sorry, all I can do is just spin there. And the others, and that first hart, it will execute this normal logic. This is the normal logic. The others just jump over here. And when they jump over here,

it's again a loop. The loop just spins away frantically right there. It just jumps to itself, right? Isn't that an infinite loop? Right? It's just that, when we have the infinite loop, Everyone, take note here. We used a WFI instruction.

This WFI instruction, is not the focus of our study. You can look it up on your own. What it actually does is put our CPU into a low-power sleep state. Because as you all know, if it just spins blindly, that consumes a lot of power.

Think about it, isn't that true? If there were no if there were no if there were no WFI instruction here, without this, then it would just be spinning blindly. Spinning blindly like that, that would consume a lot of power. which is completely pointless

and it just wastes power So after we added WFI, it actually enters a dormant state So it's not really spinning blindly It actually enters a dormant state What this means is unless there is an external interrupt to wake up this CPU,

otherwise this CPU will enter a low-power sleep state So this achieves the effect we want So that's why we say here, calling it idle spinning is not entirely accurate We should say we let it if it's not the first CPU, we let it enter a dormant state

a low-power dormant state That way, the basic logic of our leader program It's really that simple. But there are some concepts involved behind it, which we'll walk you through. Now, as for how to initialize, and how to jump into C,

we won't cover that here. The earlier exercises actually already touched on that. If we look at the code ourselves, we should be able to understand it. So jumping to a function, that just means jumping into that function. Let's take a quick look over here at

our code. As you can see, the code for this OS of ours, is kept separate from the ASM part. On our OS side, there are roughly a total of, 11 exercises in total. Now, these 11 exercises, were actually designed in a special way. So each 01 is actually

built on top of 00 as an iteration So they're really added on step by step and finally we reach this 11 step state So when you do the exercises you can also compare the two directories to see exactly what we changed at each step

which helps your understanding After all, isn't this course called step by step? What does step by step mean? Well, what was changed at each step is something you can compare to understand what we actually did So this first step, the bootstrap, is really what it's all about

We are at... Today we can probably only cover this much So this Now, in our code, you can see that our first little project is very simple, just four files. Among them, the makefile is our well, the makefile isn't really our source code.

Actually, only for the core code, there is just one header file, plus one assembly file, plus one .c file. Now, this platform, this header file, we covered the concept of platform earlier. This header file is used to hold some platform-related

hardware definitions. Right now this file is fairly small, Because right now, we don't involve that many things. We've only defined one thing. We've only defined what we just talked about. We said, how many CPUs are there on this platform?

Eight. Just this little bit to go on. That's enough. Later on, we'll gradually expand this file. And then, this .s file, it's our starting address after power-on. It's all in here. The first line of code that runs is all in this place.

Now, everything here is assembly. Now, everyone, pay attention. Our .S here uses an uppercase S. Now, why is it an uppercase S? Why not a lowercase s? As we mentioned before, the uppercase S, inside it, you can support this kind of thing.

What's this called? Preprocessor directives. The lowercase .s file doesn't have any of these things. To make things easier for us, because we'll be using the macros defined in here. You can use macros in assembly too. See? It's very much like C.

You'll notice that C and assembly really aren't that different. Now, this start.s is just the logic we had earlier. Now down here, stack here actually defines a stack region. This is very much like the earlier example. We won't say more about it.

So in this code of ours, right at the beginning, there's a piece of logic down below that sets up this stack pointer, see? We've covered all of these earlier, so I won't say much about them. There are comments alongside, which are written fairly clearly.

In case some of you can't see them clearly, so this piece of logic is exactly what we just talked about. We first read out the value of this m hat, and then, we compare this value against this 0, to decide whether we need to enter the Park state.

So actually, this statement here, is nothing more than taking this T0 value, and saving it into a special TP, into this register, for use later on. We'll talk about it when we actually use it later. Let's just skip it for now.

Then over here is the Park logic, and then we enter this FI sleep state. And we should know this code is executed by every CPU. So if you happen to be the first CPU, as we just said, for the first CPU, it will not enter the idle state.

So it will execute the following part, that is, the following section, these few instructions, only the first CPU will run them. Can everyone understand this? Can you follow this? That is to say, the code below, only the first CPU can run it.

That is, hart 0 will run it. What will it do? Only then will it, at this point it will go and execute the actual operations of this .text section. As for why it's written this way I'll leave that for you to look into on your own

And then, get this stack pointer ready These three sections above are all just preparing this stack pointer And then, finally, with a jump instruction it jumps to start_kernel this function, to execute our kernel Basically, it's booting into

our kernel Everyone follow me on that? At this point, our kernel basically, from here on out, actually, most of the code is written in C start_kernel is a C function. And the C part, is shown in a file called kernel.c, a source file like that.

Right now in there what we implement is very simple, just a while loop, because at this point we can't really do anything yet. For now, the first step is simply to — that is to say, we — and the same goes for you later on, when you write a kernel yourself,

it's the same idea. The first step is nothing more than getting your environment to run all the way to the first line of C code. As for when to stop doing that, well, at least your success has reached the first step, you can already see the light at the end of the tunnel.

That is to say, suppose, later on we will of course add some debug prints here, that is to say, but right now we have a debugger that can step through it. If, when you single-step through it, you can get to this step, then it means you're basically able to,

well, our kernel is now up and running, even though it's extremely, extremely simple. So basically, our first example, that's pretty much it. It looks like we're running a bit short on time to cover that today. There's quite a lot of material in the next part.

I'd rather not break it up. Let's save it for the next class. Let's not interrupt it. Alright then. Maybe we should just stop here for today.
