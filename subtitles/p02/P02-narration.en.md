# English Narration Script

Hello everyone, we're now starting Chapter 2, which is really our Chapter 1, since we're starting from scratch. Our first chapter is called A Tour of Computer Systems.

This part is the introductory portion of the whole course. This part is mainly a review for everyone, a refresher, basically the material from computer organization principles, the computer architecture part, covering mainly these blocks:

first, the hardware components of a computer, then program storage and execution, the design and evolution of programming languages, the memory hierarchy, and finally a brief introduction to the operating system. Let's start with the first part,

the hardware components of a computer. As we know, when you learn C, the first program you write, should be Hello World, right? I don't know if Now, as for how this program runs, how far does your understanding go at this point?

That is to say, from when we finish writing this program all the way to getting it to run, and a Hello World appears on the screen, what exactly happens along the way? Can you describe it in your own words right now? Right? But I think, that is to say,

after you've spent some time studying programming, and running things on Linux, you should have a deeper understanding of how this program runs. So here we just want to walk everyone through it once more. So, the composition of a computer,

we can summarize for you with this diagram here, right? What's on this diagram? If I asked you to draw it, that is, a computer — If you want to draw what a computer is made up of, I don't know if everyone can roughly sketch this diagram yourselves.

On this diagram, this... what is this? Right? This, we call it the bus. Agreed? That is to say, all those things inside our computer, all of them, the other things are called components. You can think of them as components. Now these components,

they're all connected through a single bus, wired together, just like a highway. So this connecting part, there are several big blocks Now, the biggest block of this thing needless to say this is the core of our computer, and what is it called?

it's the CPU Now, the CPU is actually... If I ask you right now, what is the CPU? you should still be able to say it it's the Central Processing Unit What does CPU mean? Translated into Chinese, it's called the central processing unit

right? If you go out there say, to a job interview do you think anyone would ask about these things? Maybe they will After you've learned these things in school and you go out it'd be good if you can explain them yourselves Now, this

What's actually inside this CPU? When you took computer organization, did your instructor cover this? Or on your own, what's your understanding of this part? Let me quickly walk you through it. On our side here, there are these

Inside this CPU, there are these main blocks. The first one is called abbreviated as CU. Once you're out in the industry, you'll know what CU means. It's the control unit, the control unit. Understand? The control unit is essentially the control part.

To put its main job simply, and this is something we'll cover later, it's basically feeding some stuff into an instruction parsing it into something processing it, running it, right? the control unit is over here now what is this one called?

we have an abbreviation for it, ALU that stands for the arithmetic logic unit now for the actual computation our computer a computer is meant for computing to put it plainly it's not meant for playing games so a computer when it was first invented

it was for computing so where does that computing happen? which part does it happen in? actually right here the arithmetic logic unit You can tell right away, the computations we do are nothing more than arithmetic operations and what else?

Logical operations, right? When you learned C, that addition, subtraction, multiplication, and division, what is that? That falls under arithmetic operations. What else falls under logical operations? AND, OR, NOT, things like that.

Think about it, isn't that so? So when you boil everything down to the end, it's nothing more than arithmetic operations and logical operations. So the arithmetic logic unit, that is right here. And then there's... Besides that,

there is another very important part. It's basically a bunch of things not just a single thing It's a whole collection of things And this collection, we call them all registers In other words, the data you need to process is actually all in this,

all stored in these registers and then, it's handed over to the arithmetic logic unit to perform the computation So that's what a processor is Now, besides that, we'll take a closer look at it later Now let's look at the others

This one here is called the IO bridge In practice, it's a bridge for our processor to interact with the outside world a bridge And this bridge, along with the bus, and one other very important component That's our main memory.

That's the memory where our data is stored. All connected together. Now, below this bus, there are a large number of what we actually call peripherals. Because with just these, with just a CPU and memory, the computer is basically unusable to us,

right? So we need some peripherals, like a keyboard and mouse, and a display, so we can interact with it. So everything down here is peripherals. And that's what makes up a computer. Of course, this is a classic diagram. It's basically the same as our PCs, desktops, and laptops.

And some phones have a similar organizational architecture. It might be slightly different from it, but overall it's pretty much like this. A computer is just this kind of thing. If you want to reinforce this impression later,

you can go home and try drawing this diagram yourself. Because some things, when described in words, might not be easy to remember, while graphical things tend to be easier to remember. So in our course, we draw a lot of diagrams.

Through these diagrams, you can deepen your learning and mastery of the knowledge. Now, this computer we're talking about, the one in the diagram above, this computer thing, we actually often call it the von Neumann architecture.

When you learned computer organization and design, you probably still remember this concept, right? That is, the von Neumann... This kind of organization Now, the other corresponding approach is called what? Do you all remember?

It's called the Harvard architecture. Right? So von Neumann and Harvard are the two major architectures in our computers. That is, two ways of organizing the hardware. Now, what's the difference between these two architectures?

From this diagram, we can see what the difference is. For the von Neumann architecture, you see it only has one bus to the outside. This CPU here only communicates with the outside through a single bus. Everyone see that? And the Harvard architecture,

what's its obvious feature? It has two buses to communicate with the outside. Right? So the Harvard Harvard architecture, it separates instructions and data meaning the instructions it runs and the data it operates on are separated,

kept in two memory regions, and then they interact through two buses. Whereas the von Neumann, relatively speaking, you can actually understand it as its storage area contains both instructions and data. Does everyone understand?

That's how it works. That's the most important difference between them. So, this, we can say in fact, a large number of PCs, servers, and so on, are actually based on the von Neumann architecture the main ones. So going forward, we'll actually focus on this von Neumann architecture

for our introduction. Now, this approach it uses, its main advantage, as you know, is that it only has one -- you only need to design one bus. So in this regard, as you can imagine, compared to the Harvard architecture, its hardware design is definitely simpler.

Right? The one on the right here looks a bit more complex, doesn't it? So this simplicity, but after that simplicity, the overhead of its bus it tends to be a bit smaller the control logic is simpler to implement basically, the hardware unit design is simpler

as for the Harvard architecture, its hardware design is more complex but once that complexity is there, what you get in return is higher execution efficiency because the data and the instructions don't have to compete for the same bus

with two separate paths, it's naturally going to run a bit faster think about it, isn't that right? So on some small microcontrollers, on tiny processors, the preference is to use the Harvard architecture whereas large servers and these

things like these desktop machines, compared to these small controllers, they are already relatively large processors so they use the von Neumann architecture, but it's not absolute That is to say, these two architectures also see a certain amount of convergence

in today's designs. Now, in the rest of this course, we'll mainly focus on the von Neumann architecture for our introduction. Because some of you may have seen this stuff before, or might come up and ask, "So why aren't you teaching us the Harvard architecture?"

It's not that we won't cover it at all. It's just that on this part, we do have our limitations, because we can't possibly lay it all out in full. So everyone should just know that this is how it works That's the second part of the hardware components,

which I've just briefly introduced to you. All right. Now, next, we'll move on to the second block of this part, which is about program storage and execution. So actually, that is to say, we, as we talked about at the very beginning,

a hello world, once you've written a program, how does it actually get running in the end, and how does a hello world get printed on the screen? Right? So what we're really going to do is take this part and tie it all together and walk through it again.

So, first, this diagram, it's still the same diagram from before, it's a diagram of how a computer is put together, placed here. So we, let's suppose we've now written a hello.c, so once this C source code is written, Can we just run it directly?

Of course not. Right? What do we have to do with it first? First we have to take it and generate an executable program Right? On Linux, it's called a.out By default it's called a.out Now what do we call this process? It's called compilation and linking

Right? We compile and link this thing, and once we've generated an a.out, we know this a.out, this thing, where is it placed now? It's placed, we'd say, on the hard disk. Right? So the stuff on the hard disk does it just run directly?

Not quite. We say that when we're say you're in the share directory and you type in a.out, after you hit Enter, what happens? Well, this a.out this program, we need to first load it into where? Don't we need to first load it into memory?

Right? Now, this a.out, it's an executable program. Now, an executable program, if you open it up, No matter which text editor you use if you open it up, the contents are unreadable to us. In fact, it's a binary thing. Now this kind of thing

is actually machine instructions. So these machine instructions, they are placed one by one in this AD-OUT, and loaded into memory. Then next, it starts being executed. Now at this point during execution, it involves what we just mentioned,

this control unit, CU. This control unit will go into memory and fetch these machine instructions one by one. And the fetched ones This process, we can actually think of it as the whole process is basically three steps. The first step is called fetch,

the second step is called decode, and the third step is execute. So the control unit, is really just doing these three things over and over again. Now, fetching, is called fetch in English, decode is called decode, and execution is called execute.

Now, this fetch step, basically, takes the machine instruction, and fetches it from memory into where? It actually fetches it into a register. Into a register. That code, basically, we have to translate it. Because once it's fetched in,

this 0101 computer has no idea what it means. It definitely has to translate it, translate it into an action it can recognize. Then, the third step, only then can it execute it, right? These are all fundamentals. If you don't remember them very clearly,

let's quickly go over them again. So this, just does this over and over, one instruction at a time. And this process, is when the program starts executing. But some of you might ask, regarding this execution, for example, some students say,

How is it that it's as if someone is pushing it to do this? So who is pushing it to do this? Now this brings us to, when we were just talking about the composition of a computer, there was one part I forgot to mention. That is, there is actually one important

one important thing in a computer, called a crystal oscillator. It is also a piece of hardware, and this hardware, once power is turned on, it produces things like pulses, at a fixed frequency, to drive our processor to do that repeated cycle

of fetching, decoding, and executing instructions, right? When you buy a computer or a phone, you often ask the salesperson, "What's the clock speed of this processor?" You get what I mean, right? The faster the clock frequency,

the speed is actually driven by this crystal oscillator thing, that drives it. So the higher the clock speed, the faster the computer computes. It's really just that simple. And sometimes it's not enough for us, so we say, "This frequency isn't enough, I want to overclock it."

I don't know if any of you have done that these days. You buy a computer, say it's 2GHz, and you feel 2GHz isn't enough for gaming, so you can go and overclock it. That can be done. Probably not as common now, but in the past, When you bought a computer

a lot of people would also take this whether they could modify their own computer and overclock it basically raise its frequency as a way of showing off their skills I don't know if any of you still do this now Back then, I spent the same amount of money

and bought the same computer because when it left the factory, its frequency was fixed Now, some of those enthusiasts who were more technically skilled would go into the settings on that CPU, make some changes, and raise its frequency

so the CPU would run faster, right? And that made them very happy But doing this... Actually, it's not all that great either because the frequency is high and if it gets too high, the heat can get pretty intense too Sometimes if things go wrong,

the computer just burns out. So it's a bit of a gamble for us. But from this we now know, that inside a computer, there's actually this precision thing going on. So overall, our program here, this is how it runs. This is our process of

storing and executing a program. Good. Now that we've got this concept down, we move on to the next chapter, which is about Because in the end, we still have to write programs. So as for programming languages, just understanding this hardware

really isn't enough. We also need to ask how these programming languages were designed in the first place. Because, as we know, in everything we just covered, we touched on things like machine instructions, and also things like the C language.

Now, this, this, that is, machine language, we... Because later on, what I'll be talking to you about will mainly be introducing you to some of this kind of OS-level development. And that involves low-level languages. So this low-level machine language,

what exactly is it? Our understanding of it here I'll go into a bit more detail for you So next, let's use a made-up example to get a feel for what machine language actually is as a thing Now, to keep this a bit simpler to explain, we'll invent a small computer

This small computer is very simple. As you can see, it's just the things I've drawn in the picture. What is it made of? Well, inside it, there are only two calculators And then, there's an arithmetic logic unit, and then, over here there's a control unit.

Inside this control unit, it's also very simple: there's just one instruction register, and something called a program counter, that kind of thing. This is what we commonly call the PC. What it actually does, is record where our program is currently executing,

the idea of that position. It's also a register. Now, on the outside, there's a block of memory, and this memory, its contents, are also limited. Actually, if you guys are interested in this kind of low-level hardware and computing stuff,

if you're really into it, if you're a real enthusiast, you can search online. That is to say, there are people on the internet who use breadboards— have you all heard of those? Basically, breadboards, and DuPont wires— basically, connecting wires—

and very simple electronic components, and they can literally build a CPU by hand, a 4-bit one, or even an 8-bit CPU. Because as we know, a CPU is essentially just transistors. Right? So this thing, the transistor, you can actually assemble it yourself,

and you can build one. Of course, with an integrated circuit, it's just that the transistors themselves are etched onto this silicon chip. We can absolutely wire it up ourselves to make a CPU about this big. It's just that this CPU would be very weak as well,

right? If I gave you this kind of model right now, you could actually build it out of that too. But of course, for us, this is getting into more hardcore stuff, hardware stuff. We won't have everyone tinker with that here. So let's assume we have this kind of processor,

we built it ourselves, and there are two registers in here. A processing unit, and a control unit. Now, the functionality we ask it to implement is also very simple. We don't want it to do anything too complicated either. For example, let's say it's an 8-bit computer.

So what do we mean by 8-bit? Well, you can think of it as these registers, each one being only 8 bits long. Its functionality only needs to support addition, and that's enough. So if this thing, asked you to build a CPU out of it,

you could absolutely pull it off. Because its functionality is really, truly simple. Now, think about this, everyone. If you were given these components, and asked to complete a simple addition operation, how would that actually happen?

I mean, if you write it in a high-level language like C, it's nothing more than a single line: C = A + B. This statement, Now we need to implement this statement in machine language to actually make it work Have you thought about how we would do that?

Actually, it's quite simple Think about it: suppose we have two registers and these things So roughly, how many things do we need to do? Actually, just about three or four steps Think about it: suppose we want to just a moment Suppose we now want to implement a

A equals B plus C a function like that So the first thing we need to do think about it: is nothing more than taking the data B and C and putting them into registers inside the CPU Isn't that right? Now, generally speaking, when we talk about data

It has to be put into memory first, of course. So after the CPU powers on, everything inside it is empty. So the first thing we do, probably nothing more than taking a piece of data like B, moving it from memory into the CPU first,

and then the second step, is nothing more than taking another piece of data like C, and moving it from memory into the CPU as well. Then at this point our arithmetic logic unit, can, because the arithmetic logic unit, it only accesses registers;

it cannot directly access external memory. So what we need to do, the first thing, is to take the data from memory, and move it into the registers first. Suppose this register holds B, and this one holds C. Then what comes next is to perform an addition operation.

So the addition operation, we said, is what the arithmetic logic unit has to do. So it can take B and C out of these registers, then compute them and get an A. So where do we put A? Right now we only have two registers. Where do we put A?

Actually, we just find any register, for example, replace B with A, and that's it. Think about it, right? Because B has already been used up, it's no longer useful. Think about it, right? So we put A here. Then finally, after we're done thinking,

we say we still need to do what with it? Find a way to move this A out and save it. And that's basically it — we're done. Of course, if you were to design your own small CPU, to accomplish something this simple, you really just need to implement these four instructions.

So how should we go about designing these four instructions? Why do we care about how machine instructions are designed? Because when we talk about assembly later on, it's actually closely tied to this. So it's worth everyone getting familiar with

some concepts related to machine instructions. Now, the four steps I mentioned earlier actually correspond to these descriptions up here. So the first step, as we just said, what is the first step? Think back to what we just talked about.

The first step is to take that b this data, fetch it out of memory, and put it into the first register. Think about it, isn't that right? But this memory, how do we access memory? We know that when a computer accesses these things outside the CPU,

it accesses everything through addresses. I need to know whether it's the first byte or the second byte in that memory. So, to sum it up, it's really just this: we have to access an address, and then from that address, take a piece of data and load it,

load it into a register. That's the first instruction. Now, what about the second instruction? It's similar; you just swap it out. Load it from another address into this register. Now, the third step, is to perform an addition operation.

Take the values in two registers, and after adding them together, the result, as I just said, just use it to replace one of the registers. So the third step, the final step, is to take this result, from register 0, and store it,

putting it out into memory. Now this, let's take a look at this instruction. How does it work? If we ourselves, once the hardware is done, How do we design instructions? So this because as we just said, this instruction is placed in memory,

and it's also eight bits. So we need to design how these eight bits can express this concept. So really, we have to design this. Now these actions like this, load, add, store, these are actions, wouldn't you say? This kind of action,

in our computer's terminology, we call it we call it opcode. That is, the operation, the opcode. In other words, it represents a kind of action. Now, in a computer, everything, absolutely everything, is represented as 0s and 1s.

So when it comes to this action, how do we represent it? Well, right now, let's take an instruction, suppose our instruction is 8 bits long, so one bit, one bit, 8 bits. Then we can lay it out like this. For example, we could specify that

the two lowest-order bits at the front, the 01, these two bits, are used to represent this action. Now, as you know, Two bits Actually, in our computer, that's already enough. Because with two bits, how many operations can you express?

You can express four operations. Think about it, right? 00 01 10 11 Now, these four operations on our computer, are already enough. Why? Because after we analyzed it, we found that we only need a few operations, just three: load, add, and store.

Think about it, right? So, in practice, when we use the instruction, we only need to use two of its bits. We only use two of its bits is actually enough. So we just lay out a plan. That is, we use these two low-order bits to encode the action. So for the first command, we just said the action is load. But we also need to specify who is the object being operated on. Actually, the object we operate on involves a register and an address in here. So we take the remaining six bits and split them up. These two bits hold the register number, and the four bits after that hold the address. Now what does this actually mean? It means how many registers can we address at most? Isn't it that we can only address at most four registers?

So we just lay out a plan. That is, we use these two low-order bits to encode the action. So for the first command, we just said the action is load. But we also need to specify who is the object being operated on. Actually, the object we operate on involves a register and an address in here. So we take the remaining six bits and split them up. These two bits hold the register number, and the four bits after that hold the address. Now what does this actually mean? It means how many registers can we address at most? Isn't it that we can only address at most four registers?

That is, we use these two low-order bits to encode the action. So for the first command, we just said the action is load. But we also need to specify who is the object being operated on. Actually, the object we operate on involves a register and an address in here. So we take the remaining six bits and split them up. These two bits hold the register number, and the four bits after that hold the address. Now what does this actually mean? It means how many registers can we address at most? Isn't it that we can only address at most four registers?

to encode the action. So for the first command, we just said the action is load. But we also need to specify who is the object being operated on. Actually, the object we operate on involves a register and an address in here. So we take the remaining six bits and split them up. These two bits hold the register number, and the four bits after that hold the address. Now what does this actually mean? It means how many registers can we address at most? Isn't it that we can only address at most four registers?

So for the first command, we just said the action is load. But we also need to specify who is the object being operated on. Actually, the object we operate on involves a register and an address in here. So we take the remaining six bits and split them up. These two bits hold the register number, and the four bits after that hold the address. Now what does this actually mean? It means how many registers can we address at most? Isn't it that we can only address at most four registers?

we just said the action is load. But we also need to specify who is the object being operated on. Actually, the object we operate on involves a register and an address in here. So we take the remaining six bits and split them up. These two bits hold the register number, and the four bits after that hold the address. Now what does this actually mean? It means how many registers can we address at most? Isn't it that we can only address at most four registers?

But we also need to specify who is the object being operated on. Actually, the object we operate on involves a register and an address in here. So we take the remaining six bits and split them up. These two bits hold the register number, and the four bits after that hold the address. Now what does this actually mean? It means how many registers can we address at most? Isn't it that we can only address at most four registers?

who is the object being operated on. Actually, the object we operate on involves a register and an address in here. So we take the remaining six bits and split them up. These two bits hold the register number, and the four bits after that hold the address. Now what does this actually mean? It means how many registers can we address at most? Isn't it that we can only address at most four registers?

Actually, the object we operate on involves a register and an address in here. So we take the remaining six bits and split them up. These two bits hold the register number, and the four bits after that hold the address. Now what does this actually mean? It means how many registers can we address at most? Isn't it that we can only address at most four registers?

involves a register and an address in here. So we take the remaining six bits and split them up. These two bits hold the register number, and the four bits after that hold the address. Now what does this actually mean? It means how many registers can we address at most? Isn't it that we can only address at most four registers?

a register and an address in here. So we take the remaining six bits and split them up. These two bits hold the register number, and the four bits after that hold the address. Now what does this actually mean? It means how many registers can we address at most? Isn't it that we can only address at most four registers?

So we take the remaining six bits and split them up. These two bits hold the register number, and the four bits after that hold the address. Now what does this actually mean? It means how many registers can we address at most? Isn't it that we can only address at most four registers?

and split them up. These two bits hold the register number, and the four bits after that hold the address. Now what does this actually mean? It means how many registers can we address at most? Isn't it that we can only address at most four registers?

These two bits hold the register number, and the four bits after that hold the address. Now what does this actually mean? It means how many registers can we address at most? Isn't it that we can only address at most four registers?

and the four bits after that hold the address. Now what does this actually mean? It means how many registers can we address at most? Isn't it that we can only address at most four registers? hold the address. Now what does this actually mean? It means how many registers can we address at most? Isn't it that we can only address at most four registers?

Now what does this actually mean? It means how many registers can we address at most? Isn't it that we can only address at most four registers? It means how many registers can we address at most? Isn't it that we can only address at most four registers?

Isn't it that we can only address at most four registers? four registers? Because the register codes can only be 00, 01, 10, and 11. But these four registers are actually enough for us. Why? Because in the design we have now, there are only a few registers at most.

Only two registers. So in our design, putting these two bits is actually enough. Right, everyone? So, this register only needs to hold two bits, and then for the address we set aside four bits. With four bits, think about it — how many bytes of range can we access?

Right now, because this is an introductory course, we're designing low-level development, so you'll probably need to cover these concepts a bit more. Because if everyone were developing entirely in a high-level language, these low-level things would probably get covered less.

that's the binary handling we're still playing around here I mean these 4 bits how many do you think we can access how many numbers can 4 bits represent think about it they can represent 16 numbers so that means this computer of ours

can access at most how many bytes only 16 bytes so primitive isn't it but our our complex computer here this IT world is actually built up bit by bit like this don't you think by learning these simple things and then extrapolating from them

I think if you go and learn some These are the fundamentals. Otherwise, if you don't lay a solid foundation, and then go on to tackle advanced stuff, artificial intelligence and things like that, a lot of it may actually end up hindering you instead of helping.

So what we really hope is that through this course, you'll get a sense of how computers actually came about. Now, this first instruction, we've actually designed already. That is, we can use the low 2 bits to represent the opcode,

and then bits 2 and 3 in the middle to represent registers. Then, the first register, we number it 00. So 01 stands for load, 00... and then xx? We haven't decided on that yet for now. This part is used to hold the address. So we've got that instruction figured out.

Now, what about the second instruction? It's similar to it. Think about it. The only thing we change is one spot. We just change this right here. The operation actually stays the same. Because what are they both? Because they're both loads.

Right? So all that changes is the register it operates on. Before, it was the first register... The zero-th register, called 00. Now it's the first register, which is 01. Right? What about the address? We'll hold off on that and look at it next time.

Then, our third instruction is called add. So for this instruction we need to change it slightly because the way it was expressed before isn't quite the same Here what we want to express is that the object being operated on is not

Originally the operands were a register plus a memory location Think about it, right? What are the operands now? Two registers Right? So we've re-divided this field again. That is, the low 2 bits stay the same to indicate this operation

The operation is add Then we use 11 to distinguish it to indicate add Now on this side It's about extracting these two bits and these two bits to represent the registers So the first register, that's 00 and the second register is 01

Right? And just like that, we've expressed it. Now, since we don't need, uh, these two high-order bits here, in this case, these two high-order bits are actually not used. Right. So as you can see, when we design low-level language,

not a single bit gets wasted. Don't waste anything. That's exactly what's happening here. So through this learning, once you understand these things, then when you look at the machine instructions later, I mean, for RISC-V machine instructions like this,

So I hope that through this learning you can understand how the more complex machine instructions we'll cover later are designed. Now the final step is putting the data back out. The store operation is actually very similar to the load operation,

because all it needs to express is the two objects to store: one register and one address. So we just need to change the opcode to 10, and that's it. And this way, it's essentially like saying once we've designed the hardware, suppose you're designing the software,

this set of machine instructions is already designed. Do you see that? In other words, our machine instructions So it's basically like that's how it was designed Right Now, after this thing was designed you can see that throughout our whole process

we actually designed two encoding formats Did you notice that? Right? The first encoding format where did we use it? As for the first encoding format what it expresses is it holds a register and an address So for this kind of instruction

what we use is let's call this encoding format A and this one format B Then our These several kinds Which one do they all use? They all use method A, right? That one, this one What do we use for this instruction? It's method B. This is what's called the instruction's encoding format.

Later, when we talk about assembly, you'll come across this concept as well. That is to say, the same 8 bits, there are several encoding formats in there, and which one we pick determines the information our instruction conveys.

So we've finished designing this machine instruction. Now that it's designed, let's look at how this process actually happens. Right now we want to understand how A plus B equals C actually takes place inside a computer. Because this is very low-level, the lowest level,

just really fundamental stuff. One by one, we Even a very simple operation is actually quite complex inside a computer. When you break it down all the way, it's essentially this kind of process. Let's take a look at how this process happens.

This diagram here, it's actually still the one from before. Once we finish going through it, we'll take a break. If you look at this, what is the part on the right? The part on the right is what's in our memory. You can think of it as,

basically the a.out generated after we compile. Now, as I mentioned earlier, our computer architecture, right now we're using the von Neumann architecture. So what does that mean? By now, do you all know what the von Neumann architecture is?

The von Neumann architecture means that instructions and data are stored in the same place. Do you still follow what I mean? What does this yellow part represent? The 0101 stored in this part is actually instructions. And the green part stores data.

What is it that we want to compute now? What we actually want to compute is 1 plus 2 equals 3. This one... This is in binary. 0001 — everyone knows what this is, right? This is in decimal. It's just 1, don't you think? This 0001.

And 10 converted to decimal is what? That's 2. Later on, for example, when you really do low-level development, you'll often be looking at memory. I don't know if any of you have ever looked at memory When you look at memory you often have to do this in your head

binary hexadecimal and decimal conversions Because we humans prefer decimal right? But inside a computer what you see is generally we use hexadecimal right? So now let's go more thoroughly let's look directly at binary It's really about the same

So this is 0, 1, and 2 this is where data is placed And here, this byte we're actually planning to put the result in It's putting 3. But of course it's not 3 yet. Because it hasn't been calculated yet. Now, this thing in front is the instruction.

So think about it — what does this instruction represent? Based on the design we just did, let's try to guess for ourselves. Because later on, when we do disassembly, it's really about taking the data — the binary data inside the computer —

and working backwards to figure out what it actually is. What is this thing? Based on the analysis we just did, I'll just go ahead and tell you directly. You can go home and practice this on your own. Do some training in this area.

Otherwise, when you have to do these things later, you might find it pretty painful. Honestly, I think low-level development, it's really about pushing through stuff like this. Because in high-level programming this is something you never deal with.

This 01 is the binary 010000, and what does this string of numbers represent? As we just said, let's briefly review that machine code concept from earlier. So the idea is that through this, basically what we're doing now is, you can think of yourself as the CPU.

Right now the CPU is in how does it see it? We should look at this thing the same way. So a bunch of 0s and 1s in the data, how does it look at them? As we just said, the first step of processing is fetch — fetching the code. What is the second step?

The first step is fetch, what does the second step do? that is, decoding this idea of translating it that's exactly what we need to do now suppose after you fetch this data then as the computer it's actually very simple it just looks at the last two bits

right? because we just analyzed this we've already defined it in our format these last two bits are the opcode they tell us whether we're doing a load or a store or an add right? so we just look at this 0101 thing now look at this, everyone

now let's look at this thing here this is 01 — what does that tell us? 01 is the load concept we just designed That is, loading What does loading mean? We've been a bit wordy in explaining it Loading means Reading data from memory into a register

Do you follow what I mean here? This one is load Now, what are these two bits of load? They tell us which They tell us which register to load into Right? So what does 00 represent? That's register zero That's register one Right?

Then what is this? What do these four bits at the end express? It's the source address for the load The source address in memory That is, where in memory it loads from Into the register right So what is 0100? What does this address translate to in real terms?

What is 0100? It's 4, right? Think about it. At address 4, what's the corresponding value? It's 11, right? Think about it. So if you translate this instruction into human language, what does it mean? It means load, that is, from the byte at address 4

in memory, load a value, and load it to where? Load it into register number 00. into the register Can he figure that out? Let's go through the first one slowly. After that, things will actually seem to go faster. And you'll still be able to work it out.

It's not that complicated. Because this is already a very simple computer. A computer with just 8 bits. Look, today's computers are all 64-bit. One instruction and you'd have to flip through a whole page. It's a really complex thing, isn't it?

Now we just have 8 bits. You flip through them. What it actually expresses is, as I just said, well, Okay, everyone, think about this. Now our computer starts running. Say, the moment it powers on, what state is it in right after power-on?

Right after power-on, first, everything inside our computer is reset. As for resetting, resetting these registers actually doesn't mean much, because they're just intermediate values in the process. But this program counter, the value in the PC register is absolutely critical.

That is, when the computer powers on and resets, its value always points to the address of the first instruction you want to run. Do you follow what I mean? Now let's suppose after our PC powers on, its value is definitely — and this is guaranteed by hardware —

set to 0. So of course, what does 0 mean? It means we're now going to start executing from this position in memory. Do you follow what I mean? This arrow points to this position. This arrow, if you imagine it, is really the memory location

pointed to by the PC register, which is also where the first instruction we're going to execute is located. Right? Good. Now comes instruction fetch. It's simply about running this fetch loop. When fetching an instruction, it takes...

what does it do? Now, everyone, what just happened here? Instruction fetch means taking the data from in here... and taking it out of this memory to where? It goes into our instruction register. Because only after it's fetched in can the computer...

the CPU start analyzing it. Think about it — isn't that right? Then, at this point, based on the decoding rules we just saw, it analyzes it, decoding it according to the format we just discussed. This is called decoding. This is called decode, or decoding.

Now, as for decoding, Based on the analysis we just did, this 01 stands for load. And who does 00 refer to? 00 is this one here. Think about it, doesn't that make sense? So what does 0100 point to? It's this position right here.

So for this instruction, when it executes, what action does it perform? It has to take, all it has to do is take, the data in here, the data at address 4, and load it into where? Load it into this register. Make sense? So, the moment this instruction,

executes, As we can see, in this register, its value this 01000 has been placed here. Then what happens next is easy to see. Because the first pass, the first process I described, is relatively slow. Then afterwards, what do you think the second instruction does?

Let's decode it ourselves. What does 01 mean? Still load, right? Load from where? It loads from this address. What does 01 01 mean? That's 5, isn't it? And 5 is exactly the value in here, isn't it? Which is 2, right? Isn't it? Load it into where?

loaded into 01 this register, which is inside this thing Is that still clear? So once this instruction is executed what happens? All of these things are changing Do you all see that? Because this PC is also incrementing, right?

Think about it, isn't it? Because this arrow is moving downward instructions are fetched one by one and it's this PC that keeps track of which instruction has been fetched Then it's fetched in After it's fetched in, first we fetch this instruction in

fetch it in and decode it After decoding, we start executing After execution is done this register changes, right? [1281] And at this point, [1282] the values A and B [1283] are loaded in, right? [1284] Think about it, [1285] what comes next? [1286] Next, this PC, [1287] this one... [1288] this... [1289] this... [1290] it changes, [1291] right? [1292] After it changes, [1293] it points to the third instruction. [1294] The third instruction. [1295] Just now, when we decoded it, [1296] we knew what this 11 meant. [1297] It's addition. [1298] Adding what? [1299] Adding the value in register 00 to the value in register 012.

So at this point These two values, A and B have been loaded in for us Now think about it, right? What comes next? Next, this PC this this this changes right? After it changes, it now points to the third instruction The third instruction,

after we decoded it just now, we know what this 11 means It's an addition Add what together? Add the value in register 00 to the value in register 012. This 00 is meaningless. We can just ignore it. So after we add them up, where did we say we put it?

We still put it in here. So at this point, what does the value in here become? It becomes 3. Right? And what about the last step? What does it do? The last step is that we take the value in here and save it into memory. And that is done through this

instruction 10 here. So if you look closely, it actually just does some very simple things: A plus B equals 3. 1 plus 2 equals 3 But this computer, this process, it actually, so, a computer is a pretty dumb thing It only knows how to do

these very mechanical things But where does its advantage lie? It's just fast The main difference between it and us humans is speed Right? We just talked about it for a long time, but actually it finishes in an instant Yet what it does

is really that simple We just make use of its speed to help us get things done Now, in this process, we can see The main job of a computer is really just to take data and move it from memory into the processor then process it inside the processor

and then move it to where? Back out to memory, right? So this is the core operation of a computer. Now through this process, we actually want you to understand a few things. First, what exactly does a computer's CPU look like? Take it apart and look inside,

and it's really just these things. Second, once you have this hardware, is that all there is to it? We still need software. Think about it, isn't that right? And what exactly is software? Software is nothing but these 0s and 1s.

These 0s and 1s have meaning. And who defines that meaning? Well, we do. Right? Once we've defined it, it goes into the processing units of the processor, and it decodes accordingly, then analyzes it, and it knows what we want it to do.

Everything is actually designed by us and then carried out by the machine. That's the computer, and that's the entire workflow of how it operates. So, let's wrap up this chapter with a brief summary. Basically, what we've been talking about is

How did the evolution of computers come about? Well, the very earliest predecessors of our computers actually worked in that kind of environment. Their programming was really binary programming, extremely primitive. And back then,

software functionality was also fairly simple. You've probably seen some materials about how programming was actually done back then. It was punched with a punch machine. Make sure you understand this, a hole punched by the machine meant 0,

and a position left unpunched meant 1. So when they wrote programs, they would just take that finger on a paper tape, and keep punching holes in it. you punch 01 01 01 This thing is basically our imagine this 0 means punch a hole

1 means don't punch a hole and this way after you finish punching it put it in front of the light and shine through it after the light passes through there's a receiver on the other side you get the idea? and over there it can decode it

you can go search this up yourself this is the early computers this is what's called machine language this machine language, actually our underlying hardware today still works this way but you have to Writing programs this way is

just too painful for us right? So what do we do? Well, later on we gradually got a bit smarter and invented assembly language. Now, assembly language is actually very simple. The only difference between assembly language and machine language

is that it's written in something we humans can read. But the two still map one to one. As you can see, the first line, doesn't it look much nicer than this now? load r0 0x04. What does that mean? Let's translate it. It's really just our

Of course, this is in English. Because computers were invented by Westerners, so they're explained in English. So here's a bit of a side note. If you want to learn computer science well, English is actually pretty important. Because when you're reading materials and things like that,

if you don't approach them from an English perspective, you'll find it a bit painful. But you get used to it. Now, load. We just spent a while explaining it, and this instruction is really just doing that. See? Load what? Load the memory address 0x04

into our register 0. See? So this assembly instruction is obviously much easier to read. When we write it in assembly language, it's already quite close to human language. So the second machine instruction, is simply load r1, 0x05.

The third instruction, as we just said, is addition. It adds the two registers r0 and r1, and then stores the result in r0. Right, everyone? we members just have to learn assembly So you all still need to get used to this kind of language.

And the fourth instruction, is simply to take the final result from r0, and move it to 0x04, 06, that memory location. And the data, is simply 0120 placed there. Through this kind of thing, when we talk about assembly later, What exactly are these concepts we've been talking about?

It's really just telling us here we put a byte, put a byte, put a byte, and in the end, this part becomes assembly language. We can no longer see our machine instructions. But this process, we say, hasn't really been lost, because our assembler

did that for us. Right? After studying compilation, you should have some understanding of this. Now, as you know, writing in assembly is still painful. Because, first of all, as we just saw, assembly and machine instructions It's basically a one-to-one result,

right? If your program is written to be very complex, your assembly code will end up very long, and hard to read. This stuff— these four instructions are still easy to read right now, but that's why in the end we ended up with C.

C actually isn't a very abstract language, but it's already much easier to read. Take a look at this C code. What does it express right now? This C code— actually, compared to this code snippet here— it basically corresponds to it.

Can you all read it? Now, when we look at C code, have we already seen the essence of the computer? That is to say, when you see this piece of C code, in your head, can you picture that it is really just 0101? Look at this. Now we know that

when we define an array of bytes — this byte array, an array of three bytes, with value 120, what is it actually in memory? It is actually something like this. Can you picture this thing now? You must be able to picture this. When we execute this statement,

What is it doing? Actually, this one is still fairly basically still one-to-one, but written in a more complex way. In fact, one statement might turn into many assembly instructions. So this statement is actually similar to which instruction?

It's actually equivalent to this instruction, right? And then this assembly actually corresponds to this machine instruction, right? And so on, it's just that this instruction, taking this instruction as an example, is a bit more complex.

As we know, one high-level language statement actually corresponds to several assembly instructions. Can you see that now? Let me clear the screen. Let's take a look. Let's pay attention to this one. This high-level line. Actually, one high-level line corresponds to two assembly instructions.

Which two? Add and store. Think about it, isn't that right? Because this thing, arrow2, actually corresponds to which block of memory? It's actually this block of memory. Right? So these two assembly instructions actually correspond to this one line of high-level language.

And what these two assembly instructions correspond to are actually these two machine instructions. So from here you can see that high-level language is really getting more and more refined. Alright, so this is the evolution of our languages.

From machine language, to assembly language, to high-level language. So high-level languages might... Now there are plenty of high-level languages. C is actually still a relatively low-level high-level language. If you're developing something these days...

like Java or something... languages at that higher level... the abstraction is even more intense. So the real point of this course is to get everyone... to look downward. Once we can see what the essence of a computer really is...

then this course has achieved its goal. So at this point, let's say... when you look at that printf statement... have you ever wondered... what's actually behind it? How does this process actually happen? It's really a reverse...

a backwards process. Right? I won't go into that here. Just now, what I mainly introduced to you was how programs developed from assembly language from machine language to assembly language to high-level languages, such a development process.

Now, next, I will introduce the fourth part of this chapter, which is the memory hierarchy. So, on this, as we know, in what I just introduced to you about the composition of a computer's storage devices, we saw that several parts are related to storage.

The first part is registers, the second part is memory, and the third part is the hard disk. So these parts are all hardware-related parts, the parts related to storage. So these parts, during the running of a program, Both play a very important role.

Between them there is a very important pyramid structure. Let's take a look at it together. Now, this diagram is also an important one for our hierarchy. Take a look, everyone. In this pyramid diagram, the higher up you go, the closer it is to our processor.

For example, registers and caches. The further down you go, the further away it is from our processor. Now, for this part closest to the processor — the registers and caches — their characteristic is that they are relatively fast,

but their capacity is relatively small. For example, take typical registers. In various processor architectures, there are at most only a few hundred registers. But this part, the manufacturing cost is relatively high. And the further away it is from our processor,

its processing speed gets faster and faster, slower and slower, while the capacity can be made larger and larger. And its cost can also be made lower and lower. So this is the hierarchy of our storage devices. Now, from a programmer's point of view,

the reason we understand this hierarchy is mainly in the process of developing programs, so that our programs run fast, we can take the data we operate on frequently and place it as close to our processor as possible, for example in memory, cache, or registers.

That way, our programs can run very fast. This is a kind of optimization technique. So I hope everyone can also get an understanding of this diagram. This is the hierarchy of storage devices. The last part, as the final section of this chapter,

the last part, as an important part of our entire computer system, is the operating system. This is also an important topic that I hope everyone will study in this course. So this is our last operating system. Now, as for this operating system,

we know that in in our entire computer system, at the very bottom we have our hardware, and at the very top we have our software. Now, as software developed, software split into two parts: one part is the operating system, and the other part is application programs.

Now, between the operating system and the application programs, there is an interface. And this interface, we call system calls. And between our operating system and the hardware, there is also a layer of interface. And this layer of interface,

we call the instruction set architecture. So the instruction set architecture, which is also what we'll cover in detail in the next chapter Now, the operating system, which is the main focus of this course, its main functions

can be boiled down to two. The first is to protect the hardware from being abused by out-of-control software applications and programs. Because if we let applications access hardware directly, given varying levels of programmer skill,

or unexpected exceptions in a program, the hardware could be damaged. So one of the operating system's main functions is to prevent applications from accessing hardware directly, and instead go through our system calls, with the operating system accessing the hardware on their behalf,

so the operating system serves to protect the hardware. On the other hand, going upward, it provides applications with a simple, consistent abstraction interface, which is exactly the system call. so that applications can access the operating system through system calls,

and thereby indirectly access our hardware. Alright, for today's class, that's all I'll cover for you.
