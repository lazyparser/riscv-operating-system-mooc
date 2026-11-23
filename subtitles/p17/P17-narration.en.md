# English Narration Script

Okay, so let's start today's class right now. We'll continue with Chapter 7 from last time, HelloRBOS, and keep going from there. Last class, we mainly walked through the system boot process, right? We know that this Virtual board simulated on our emulator has 8 cores in total.

Now, out of those 8 cores, in practice, in our usage, we only make sure to use one of them and that's enough. So next, what we're talking about is when it comes to developing an operating system, when you guys debug programs, the thing you probably like most,

for instance, if you want to see how a program is running, is through printing, right? printing its logs. Well, with this board, honestly, we didn't have a proper way to print this stuff in the beginning. So, to make things easier,

for our later debugging, we also need to take some of the things inside and print them out. Just like when we wrote our first program, Hello World, so how do we print this thing out? We actually make use of a serial port device on this board

— this kind of serial port device — to print the information out. Let's take a look at what method is used to print it out. Now, this diagram, I believe I've already shown it to you before, that is, in embedded development, Now, in the typical case,

this here is our development board, the target board. So what we do is, normally, on this host machine, we do the development, but the actual program runs on this target board, right? So this, what do we call this kind of setup? As mentioned earlier,

what is this kind of development environment called? It's called a cross-development environment, right? In embedded development, this is a typical runtime environment, a typical development environment. So now, what we do here

That is to say, this development board we didn't actually use a real physical board what we actually used is an emulator to simulate an emulated board that is, QMU is used to emulate it Now, actually, the way QMU runs compared to a physical board

the difference is really not that big Now, for this kind of embedded-style board, since the devices on it are limited it's generally not like our PC environment, which comes standard with a monitor or something like a screen so it's hard for us to, that is,

print our debug output right from the start onto this kind of screen actually, a screen is a very complex device So what do we do? Actually, when we debug Generally, we actually make use of our host machine Our host machine has a screen on it

Or you can think of it as something on the screen running a console, this kind of concept So yes, it does have a screen So we can actually make use of this board On top of it Through this serial port cable That is, this red cable, we connect them up

Through this serial port cable Then the running status of the program inside it Actually, through this serial port cable For example, if you want to print Hello World You just use this serial port cable You send this message through this serial port cable

and print it out on our host machine And then, through a program running on our host machine a serial port receiving program which you can think of as a client and then it prints this content onto our screen this way we can also see the internal debug information

right? in fact, the very earliest kind of on-site debugging was done exactly using this kind of approach because back in the beginning, if you wanted to hook up a display on this, that was actually quite a complicated job so we can use this serial port,

this kind of relatively simple device, and output the content through it and in this way, we actually achieve the effect of both displaying and debugging the output. right? So this red-colored wire here is our serial port cable

Now of course, what we're using right now is an emulator, so inside it there isn't really a physical cable connected but you should imagine that inside, it actually emulates a physical serial port cable So this kind of serial cable,

we say it's actually very simple. It's just one receive, one transmit. It's this diagram here. With this diagram added, the meaning it actually conveys is this: You can take what we just talked about on this side, treat this end as this thing here,

and then treat this side as that thing, and then the two sides are connected by one cable. Inside this cable, actually, there are actually two wires. If you cut open that serial cable, you'll find there are at least two or three wires inside.

So this one-receive-one-transmit, that is, the TX on this side, in technical terms, is the transmitting end. That means it's for sending. So the transmit side, and then over here, this side is a receive, RX. So T stands for transmit.

And R stands for receive. That makes it easy to remember and understand. So what it sends here, comes from here to its receiving end. And on this side, if there is information, you can also send information over from here. Think about it, isn't that right?

For example, when you press Enter on your side, or type some characters, how do those characters get transmitted to this board of ours? Well, they actually go through the transmit direction here. So this serial port actually has two wires, one for sending and one for receiving.

It can send and receive at the same time. Now, normally for our host machine, before we use it, people usually go and configure a serial port. Right? If any of you have done real board development, So all of these things need to be configured.

Right? Now, why do we need to configure things like those PLIC settings here? These things are actually related to the serial port device. We'll give you some introduction to that later. Now, over here, it's basically like we open one on the host machine.

Like I just said, you open a terminal on the host machine. So this information, is actually printed on that terminal. Right? This is a standard embedded cross-development environment. Now, currently, what we actually want to do is,

on this emulator, that is, effectively on this board, we want to take the serial port device on it and put it to use. We need to get it working. And then, using this serial port device, to pass the data over and print a hello on ours

So this first step of ours you see, actually in this whole process of ours, this should be the second step The first step is the entire initial boot process The second step is to say, we want to print a hello out on it Once we have this thing,

everything after that becomes easy because we can see the results of the program running right? Of course, we can also use GDB to debug right now, but a lot of the time, we actually prefer using this kind of printing-log approach to look at things,

because it's a bit more efficient that way Now, as for how to get this serial port device put it to use and what exactly UART is these are the concepts we can briefly introduce to you here so this UART the full name is called it's actually an English acronym

namely Universal Asynchronous Receiver and Transmitter so it actually expresses a few concepts that is, it is a Universal means unified that is, historically speaking, it took quite a lot of things and made them fairly general-purpose

and unified and then it embodies a that is, it has the concept of asynchronous Asynchronous, the concept of asynchronous and then Receiver and Transmitter which is what's written down below here It can send and receive at the same time

That diagram we just saw Everyone has seen it It has two wires for sending and receiving Right? It sends and receives simultaneously So we often just call it the serial cable Now, the serial cable mainly because it has one characteristic

which is the concept of serial So what do we mean by serial? That is to say, because when it sends and receives, there are only two wires, right? Think about it. If we take one of the wires as an example, then what it means is We only have one channel

that can send and receive data. Now, when this data is sent and received, at that time, it actually, what we call serial, means that, its, the data inside it, is sent out bit by bit, one bit at a time. Now, more specifically, that is to say, for example, let's say,

we want to send a character A. We know that this character A, when represented in our computer, it's ASCII, right? It's actually, an 8-bit piece of data, isn't it? As for what exactly it is, I can't remember exactly, I think it was something like sixty-something, or whatever it was.

So actually it's an 8-bit piece of data. Now, an 8-bit piece of data, as represented in our computer, is simply 8 bits. 8 bits, a sequence of 0s and 1s in those bit positions. I'll just make up the actual values here, 12345678.

Say we have a piece of data like this. Now for this piece of data, how does it actually get sent out over the serial port? Well, this is serial transmission. It's sent one bit at a time. In other words, if it's 8 bits, then you have to send 8 electrical pulses,

meaning the signal level has to change 8 times. A 0 represents a low level, and a 1 represents a high level. That is to say, it's basically like And also, what it says here is that When it transmits, over here it's let me see if it's written here

that is to say, it actually sends it from the low bit to the high bit. So if the data I've written here in our computer is a value like 11101011, then when it transmits, it sends the 0 first, then the 1, and that's how it gets sent out.

So, that is to say, the voltage level on this wire, if you measure it with that meter, you'll find that at the very beginning it's a low level, that is, the first one is a low level. What we usually see on the oscilloscope is a low level.

Then there's a high level right? And then another low level And then another high level So here it's 0101 That is, low, high, low, high And then going further along So if this is a timeline the bits are sent out one by one like this

And the receiver also receives them bit by bit Every time it finishes receiving eight bits it knows that's one character and then converts it into an A That's the processing flow You can picture it, right? So this since it's sent bit by bit

it brings in a concept the concept of baud rate This concept We also hear this concept quite often, meaning that when we configure it, we also have to configure this, because the baud rate on both sides must match — the transmission frequency has to be the same,

so that the receiving side and the sending side can line up with each other. Now, what is a baud rate? Its unit is actually bits per second, meaning how many bits per second, that is, how many bits are sent per second. So do you understand now? When someone configures

a baud rate like 115200, what does that mean? It means it sends 115200, that is, 115,200 high-and-low level transitions per second. Each transition is one bit, so 115200 means 115,200 bits per second. That's what it means per second

This is its sending frequency The bigger this number is the faster it sends right? So this and moreover, what's shown here is serial Let me explain to you why, what serial means That's what it means So parallel actually means it's equivalent to saying

parallel means right now there's only one wire right? So parallel means if we used a parallel approach to send eight bits then you'd probably need eight wires sending them at the same time, sending all eight bits over to the other side in one go.

That is what we call parallel. Now, with serial, you can see its advantage is quite obvious, namely that it saves material. It only needs one wire, right? With a parallel port, at a minimum, you would need eight wires, and all of that costs money,

because every wire, each of those lines, costs money. So for this serial port, we can say that physically speaking, it is also very economical and practical, good enough for use. Now, the second point, it is an asynchronous concept.

So what does asynchronous mean here? This brings us to something we will introduce to you later, which is that, because in this as we just saw, when it sends and receives data lines, it only has data lines, and it does not have

In other words, when we send data on both sides, there is actually a concept involved, which is called synchronization. What is synchronization? It is basically like this: if I want to interact with you, we need to have a signal to tell you,

"I'm starting." When you see that I've started, then you begin receiving data, and only then can you correctly parse it. Suppose I don't do any kind of synchronization with you, and I just start sending data to you. Then how do you know when to receive

and when to send? Think about it, right? This is what we call the concept of synchronization. Now, for synchronization, there are many ways to do it. One way is to synchronize using a clock. That is to say, everyone basically has a watch in hand,

and we sync up on the time. For example, we agree that we'll start sending data at one o'clock. Then at one o'clock you start receiving the data. In this way, we're using the clock. So this kind of approach Usually, you need to add a clock line

so that after synchronization on the clock you can then start sending the data. Now, if we look at this window, it does not need a clock line. It is just a single data line. So how can you use just one data line to synchronize the data?

That comes down to when it sends the data, there is a data format issue. That is, the data it sends is not simply just sending A, B, C, D. It actually has some special markers to indicate that a piece of data has started being sent,

and that a piece of data has finished. I think you all understand what I mean here. So through this data protocol, you can also achieve a synchronization effect. Later on, we will also show you its specific handling method. So this involves

agreeing on a communication protocol format. So on the software side, there are some On the hardware side, it's simple, just one wire, but on the software side, you have to define some special specifications to make the whole thing work.

So this is the concept of asynchronous. Now, full-duplex is easy to explain: it means it can send and receive at the same time, because as we can see, it actually has two wires, one receiving wire and one transmitting wire, right?

So it can achieve full-duplex. So this is what we call UART. After all that talk, there is actually a standard specification. If you're interested, you can look it up, there's an international standard specification for UART defined online.

We'll just give you a brief overview. Okay, these are the characteristics of UART. Really, these are just some of the definitions of UART. So those were the definitions just now. Now, physically speaking, let me give you a quick look,

it originally had this kind of connector standard. Nowadays, people probably use it less often, because in many cases USB cables have taken its place. Actually, the earliest UART cable was this standard type, the 9-pin cable.

Now, on this side, this is of course called the male connector and the female connector. The male connector has those protruding, pointed pins. The female connector is actually recessed, with sockets. And the two sides can mate together.

That's how they achieve a physical connection. Now, the pins on it are also specified by a standard, defining what each pin means. So the commonly used ones, in practice, even though there are nine pins, in reality we only use

Generally, three of the pins are used. One is for receiving, one is for transmitting, because as we just saw, transmitting and receiving is two wires, right? And then, there's actually also a ground line. So just get a rough idea of this.

If you have an actual physical board, you can go and take a look. You'll find this kind of interface coming out of it. This is its physical interface. Now, the more important part, as we just mentioned, is that from the software side,

since we still tend toward some we still focus more on software concepts, even though the physical side is simple, sending and receiving data requires a communication protocol to define its specification. its transmit and receive behavior, and its synchronization behavior

so it defines a communication protocol now, this UART communication protocol, is actually very simple let's take a quick look at it that is, what the process of sending and receiving data actually looks like so this figure, the one on top here,

we can actually understand it as, let's say this side is a sender, TX, and this side here, let's say it's an RX, and this here, what it actually expresses is a timeline that is, if this side is time, if the horizontal axis is time,

then the vertical axis, represents, its This thing on this line, the level going high and low, it can actually be understood this way. So we say, normally, for example, on this line, how do we, that is, it's nothing more than either transmitting data,

or it's not in the state of transmitting data. These are two things, and we need to tell them apart. Think about it, isn't that right? So how do we tell whether it's currently transmitting data, or not transmitting data? Well, we define it like this:

when this bus, which is really just this line, when this line is in the idle state, that is, it's not working, when it's not sending data, the specification requires this signal line to be at logic one. What do we mean by logic one?

That is, if you measure it with a multimeter, it actually guarantees that on this line of this window, it sits at a high level. So-called high and low, of course, is a relative concept. It doesn't mean a specific number of volts.

It might just be relative to some value. If it's higher than that, then it's a high level; lower than that, and it's a low level. So this, that is, after it's pulled high, it actually stays in the pulled-up state all the time.

You just learn, so, how do we know it's about to start sending data? So it the transmitting end will actively pull this high level down pull down for one bit So how long is this one bit? Actually, you can work it out. Because, as we said, once the baud rate is set

you know that this duration of one bit is actually, for example, 115200 so the duration of this one bit is 1/115200 of a second Think about it, isn't that right? So how long does this one bit last? Because the horizontal axis is time,

so how long does this one bit last? These can all be measured. So at the very beginning, it sits at a high level, which means it is in the idle state So once we're ready to start sending data how do I tell the other side that I'm about to start working?

Well, it pulls the line low that is, it takes the voltage from high to low and holds it for one bit of time, and that signals the start. That is the start bit. It means exactly this: the sender first sends a low level, a zero, to indicate the beginning of the character transmission,

right? Then, for the actual character being transmitted, as we know, generally, we just talk about one byte, one piece of ASCII data, that's what we call the transmitted data. Now, this gray part at the end, that means it's sending this data

Now, this data, according to the rules of the protocol, has what's called a data length This data length can be 5, 6, 7, or 8 bits But in general, we usually just use one form, with the length set to 8 bits The ASCII standard uses 8 bits for everything

So when we send an 'A', it takes up 8 bits In other words, this entire whole process can actually only send one byte Does everyone understand? Every time it sends a byte, there has to be a process like this So, we send 8 bits Once that's sent,

it forms one 8-bit unit. Then, the rule is to send the lowest bit first, and the highest bit last. The example I gave you earlier, I was actually explaining this very concept, that is, with an 8-bit piece of data, each of its bits —

do we send the low bit first, or the high bit first? This is also something that has to be specified in the protocol. And then, after the data has been sent, it will be followed by one additional bit. This bit, we call it machine parity bit

It's called parity in English. Now, this parity bit, what it really does, its actual purpose is to, the original idea behind its design, is essentially this: it helps us check whether the data got corrupted during transmission. For example, as we all know,

circuits can be subject to interference. Once electromagnetic waves get disturbed, a one you sent from your end, might be read as a zero on the other side. You see what I mean, right? There can be some instability, causing some interference.

So how do we detect that the data has been disturbed? A problem occurs. So we introduced a concept of parity bits. That is to say, this parity check well, in practice there are a few kinds. One is no parity check, that is to say,

for instance, in general, we might assume that this, normally, say, when you connect your board to yourself, just hooked up like this, if there is no strong electromagnetic interference in the environment, the data generally won't go wrong; it's

still fairly stable. So if we save one bit, the transmission speed will definitely be a bit faster. And this one You can also configure it to that is, not to do parity checking. Usually we seem to configure it this way, without parity checking.

But you can also configure it to do parity checking. Now, for parity checking, there are two types: odd parity and even parity. So what is odd parity? Well, that is, if among the data bits, the number of ones is even, then the parity bit is 1.

What does that mean? It means that as the sender, say you want to send out a piece of data, and in the data you are sending, within these 8 bits, say there are 7 ones, then that means there is an odd number of one bits. So if the number of ones is odd,

this part here is written wrong, sorry about that. This should probably be the odd it should be odd parity, down here it says even, up here it should be odd. That is to say, if the sender sends this data, and there are 7 bits that are 1 in it,

then that means the number of ones is odd, so the sender, on this parity bit position here, sets it to 1, to tell the other side that in the data I'm sending, there are actually then the number of 1s is actually odd So after the receiver gets it,

based on this bit, it checks whether your odd parity data matches what it says the expressed meaning is the same or not, doing a simple check. Everyone understands, right? This simple parity check, it's just a simple check. Actually, it's not completely accurate,

but after all, it serves a certain checking purpose. Now, this even parity, is just the opposite. And so on and so forth. What else is there? The parity bit is always 1. Why is the parity bit always 0? Actually, it also has a special purpose.

I won't go into the details here. So what this means is, after it finishes sending the data, it usually adds a parity bit as well. Or there might not be this bit at all. So after sending is done, how does it indicate that one byte has been completely sent?

Because we have a beginning, we also need an end. This is the concept of synchronization. That is to say, before I send, I wave my hand, and after I finish sending, I also wave my hand, telling the other side that it's done. Right?

So for the start, we signal the other side with a low level a single-bit low level to tell the other side. So how do we tell the other side that it's finished? It's actually on this line, we need to actually produce a high level of one bit, one and a half bits,

or two bits and this is configurable as well. So in this way, we tell the other side that one byte has been sent, that one byte has been sent. So our entire..., that is to say, once you say you want to send eight bytes, or say ten bytes,

then for each byte they all have to go through such a process. So this is one plus that makes one frame. This frame, a frame in UART, its this actually has a start and also adds a parity bit, and together these make up this one frame.

Does everyone understand? So this sending process is actually not as simple as you might imagine. It's not just that you send data, pure data. It actually has some bytes with special meanings in it, which serve some synchronization purpose,

and a verification purpose. You get what I mean, right? This is the communication protocol of the serial port. Then the transceivers on both sides have to follow this protocol, doing some checks on the data on the line, so that the data is

correctly sent and received. Now, as for this information, when we use this serial port transceiver, what it actually means is that, for us as software engineers, we need to configure it. You understand that, right? You see, as I just said,

the parity bit can be there or not. So whether you want it or not, that's up to you to decide. Then, on the peripheral interface, there are some special registers, that let us configure it. So, before we correctly send out a piece of data,

first there is a certain initialization process. That is, we need to configure some devices. All of our hardware devices, including writing drivers, actually follow this same idea. Before using them, you have to do some configuration,

do some initialization. Now, why do we need to do initialization? There is definitely a reason behind it. So we look at this interface, why is it done this way? I've given you some introduction here. This is the concept of the communication protocol.

So next, let's take a look at when we actually do the programming, how exactly we operate this thing. Now, as we mentioned earlier, So this, we this NS16550A, actually it is the emulator we are using now, the one it emulates, this serial transceiver,

is exactly the model of this device. This model, by the way, is a real model. On those much larger machines, they use a rather common serial transceiver model. Now, this serial transceiver, its technical manual is also available.

This technical manual is also, we'll take a look at it together with everyone in a bit. So, for us, that is, when it comes to operating an external device, namely this serial transceiver, then what is it that you need to operate?

Think about it. It's basically to program it. When we talk about programming an interface device, what we're really doing is controlling some of the registers inside it. Think about it — isn't that the case? So when we program,

as for these registers, how do we access a register? For the general-purpose registers on the CPU, we access them all by name. Right? Things like X1, X0. You still remember that, right? But these peripheral registers — there are so many of them.

How do we access them? Moreover, the registers of each type of peripheral are different. We can't possibly give them all unified names. It's not like the CPU, which is just a single block. So what we actually use is, what's called memory mapping.

What is memory mapping? That is to say, we take every peripheral, across our entire physical address range, and assign it an interval. For example, take this UART device. We see that in well, this table here, I dug it out of the QEMU source code.

If you're interested, you can go and look at QEMU's simulator source code. What it does in there is, for each type of peripheral, and these are all peripherals in here, which we'll introduce to you step by step later on. So the UART device is the serial port device,

and it is one kind of peripheral in here. Now, for all the registers of this peripheral, it assigns an address to each register, and these addresses are laid out linearly across the memory range. This is a physical address. And this physical address,

starting at the position 0x1000, From this position starting from this address we reserved this chunk for our use for our use Now, this chunk was given a range called Linux100 That is, from this address from this physical address to this address

this range was given And for this range we mapped it to this to these registers of this serial port receiver That's the idea We So let's make sure we understand this diagram from earlier When we were thinking about that boot process earlier

we also looked at this diagram, right? Back then, which things did we only introduce in this diagram? back then, we only introduced one ROM and one RAM so our memory is here so the memory's for example, this memory of ours is as large as 128 megabytes

and for these 128 megabytes, we actually mapped it to our entire address from Linux 8000 to Linux 8800 in this range this range is actually 128 megabytes so in terms of the physical address, from the physical address of our bus,

the physical address of the bus, from Linux 8000 to Linux 8800 in this range, it is given to this memory for the memory to use you can understand it this way So this memory of ours, this physical memory, if the base here is 0, then 0 gets mapped to 0x80000000.

You all get what I mean here, right? Of course, when we program, as far as we programmers are concerned, the address we see is this one. You understand, right? So when we compile and link, we access based on this address, the first byte of our memory,

that is, the first byte in our memory actually corresponds to 0x80000000. When you did the exercises before, you should have come across this concept already. Right? Now, with the same concept, take this serial port device, the registers that are accessed on it

also gets mapped to our unified physical address space And this mapping maps to 0x1000, a range like this. A range like this. This range actually comes from here. Now, this so-called mapping process, is done for us by the physical hardware.

Our software doesn't need to configure this. Of course, over here we're using an emulator, so it's implemented in software, but on real hardware, you can think of it as the circuit board being designed with the connections wired up.

And the address range configured up here is reserved for the serial port device's registers. What each of these registers represents is specially defined. Let's come back to that later. So when we access these registers, we access them through these addresses.

That's actually what this means. So this is what we call the first step to accessing this register: first, you need to know its address. Now, on this, let's look at our code together and see where we see the concepts I just mentioned.

I've opened it right here, I didn't have time to open it earlier— the code we gave you, I've opened it. In this code, for example, today we covered the operating system's— still this one, 01 hello rvls, right here. So, where do we see this?

Where does this show up in our code? This thing we just talked about, the concept of serial port mapping, we can open up this file, uart.c, Can everyone still see it clearly? It's a little bit dark. So as we can see, that is, we should

first look at this file, or rather, the code in this file, platform.h. Now, this platform.h, this is where we define everything related to our development board, all the physical characteristics and properties. The concept of a platform,

which we covered earlier, You all still remember this, right? platform actually corresponds to essentially our this development board, or you could say this SoC, the concept of it. So in here, for example, earlier we defined the concept of eight processors,

and we've kept that here. In today's exercise, we've added another concept, namely, we first defined a UART0 as this 1000. Now why define this? This is exactly what we just talked about — it's the starting address of its mapping.

So for all the registers, we place them according to this base address. to do a corresponding operation. Because here we defined a UART_REG macro. Now, as you can see, this macro is actually inside this It takes a parameter REG,

and then it actually works on UART0, that 1000 range we just saw, using it as the base value, and adding an offset to get the address of a corresponding register. The macro is used to get the address of a corresponding register,

that is, one of the registers on UART0. That is the effect of this macro. This macro is in fact intended to achieve this effect. And this REG parameter, is actually passed as these macros. So all these macros these macros, actually correspond to,

one by one, the control registers on this UART serial device — these control registers. Their offsets are 0, 1, 2 respectively. So in total, you can think of it this way: on the UART device, it provides 8 registers in total — just 8 registers.

Then later on, we use these macros, pass in the base, and we can get the address of each register. Then, through that address, we can access the register. So reading and writing the registers, there's no problem with any of that.

Now, this UART serial device, what this device is actually What are these registers actually used for? To figure this out, we need to look at the specific datasheet for this serial port device this datasheet, And we've already provided this datasheet for you,

we have also provided for everyone in the repository you downloaded. You can go in here, there's a directory called reference, which means reference materials. Inside this reference materials directory, there's a document called TD16550PDF.

This document actually contains the definitions of its registers. Of course, that standard data— that is, the datasheets engineers read— is really similar to this kind of document. It's this document. Let me also show it to you first,

so that if any of you do low-level development in the future, So this datasheet is something you'll need to consult often. In other words, information on all the components is actually included on it. This NS16550 serial port device,

its standard document is actually a document that looks like this. But this document is in English, and the content is quite engineering-oriented. So we won't use this document to introduce it to you. We actually picked up a document,

a document that someone on the internet specially summarized for us. This document, it's much easier to read. Let me put it this way, it's easier to read. Its, it actually takes that kind of standard engineering-oriented document,

and converts it into a reference document that's easier to look things up in. So let's take this document and use it as a reference for this programming exercise. Let's take a look at it together. In other words, including the one up here on my screen,

this diagram is actually also from that resource document I just mentioned — I extracted it from there. So let's take a quick look. Over here on this diagram, of course, we should explain what some of it means. For example, over here it says

A0, A1, A2. What does that mean? What it actually means is the offsets of those registers. that offset Because it only provides 8 registers, and those 8 registers As you all know, to represent 8 registers, you only need 3 bits and that's enough.

Think about it, right? So the encoding is simply 000, 001, 010, and so on. Right? With 8 such encodings, you can express 8 registers. Of course, in actual use, you don't use this number directly to access them. Instead, you have to add the mechanism we mentioned just now,

the 1000 mechanism, to get its actual address. That's how you can access these registers. So these are the 8 registers. Now note that, as you can see, these registers That is to say, there are some registers that actually appear more than once.

0000000001 It has entries that show up repeatedly. Now, these repeated entries basically mean that, this register, it has some shared or multiplexed functions. And on top of that, each of these registers, is further split into two modes,

namely read mode and write mode. So what do we mean by read mode? It means that when we read from this register, what information can we get out of it? That is essentially what it is telling us. And when we write to this register,

what effect does that achieve? It is giving us that kind of explanation. So when you look at these registers, take this register zero, for example, what it actually expresses is this. When you read it, it is used to read some kind of

value from the receiving register. We'll come back to this later. But when you write to it, it can have two different functions. One is writing, which means writing into this transmit register. And then there is another one, which is to enable something or other.

So what this means, in practice, is that when this register is written to, it has two different functions. So how do we... Well, these two different functions cannot be enabled at the same time. So it will set some other bits to achieve, that is to say, selecting different functions

which we'll see later when we program I'm telling you this here so that when you read this document on your own you understand the concept Let's first take this diagram and just briefly go over it Anyway, just know that there are eight registers

and each register has two different modes read mode and write mode You can read from it and you can write to it And roughly what they mean we'll look at in detail later What we just did was get a sense of the registers of this serial port device and some of the

basic concepts of how to access it As for the actual code I just showed it to you right here Correspondingly, we defined some macros here to match those, to represent the values of these registers. At the same time, we also defined a set of

a set of functions, like uart_read and so on, uart_write and so on. Now, for this set of functions, as you can see, they are actually converted through these macros mentioned earlier. So this one, calling this macro is essentially like taking from this register,

passing in a register value, and it will take that value and fetch it out through this, just like accessing a variable. When you read it here, you're accessing a register. Now we just treat it like accessing a variable, and that works the same way.

Writing works the same way too. Take a look at this. If you can't quite follow it, We can talk about it more after class. The code is still fairly simple. It's just this part. We basically take this behavior, and use these two — actually just two functions — to implement it.

Now, when it comes to using this serial port device, there are mainly a few parts. That is, first of all, we know what we need to do with it: we need to initialize it, right? Now, what initialization means, basically, is setting that

— for example, the baud rate, setting what we just mentioned, like the parity bit, and all that. This way, Only then can this device work properly. This is something we must do. Now, for the corresponding code, we can take a look at

in our code, this UART init function. This function will, during our boot process, be called. We'll look at it together in a moment. Now, here I've captured a screenshot, and I'll briefly go over it, that is, roughly what this initialization

needs to initialize. So, first, we first take the interrupt and the interrupt functionality inside this device We've disabled it. Now, this serial port device, when will the interrupt actually be used? We'll use it later on. We don't need it for now.

For the moment, we don't want to introduce the concept of interrupts, because we haven't covered interrupts yet. All we need to know here is that we've turned the interrupt function off for now, and we're not using that feature.

Now, down here, we're actually setting the baud rate of this serial port. We already covered the concept of baud rate earlier, the transmit and receive rate. We have to configure that first. Next, what are we actually setting here?

We're setting some of its data and control bits. These concepts basically mean that This piece of code is actually, basically, what we're doing is setting this thing up Now, of course, this interface of ours is set up on the client side

but we also need to set it on the board because our board doesn't have an interface so we set it up through code instead Same effect. What's below this, is actually setting up these, what are they, these error-correcting code bits and so on,

these kinds of concepts So how exactly is this thing configured? Let's take a look at the code together. This one, right here, just this part right here. Let's look at this first. Here, it's disabling interrupts. I'll skip that part.

Uh, let's first take a look at this piece of code. There's a detailed explanation above as well. Let me just quickly go through it here. Uh, this thing, we set this thing up, that is to say, uh, it works like this. There's this concept behind it,

that is to say, uh, when setting the baud rate, we know that this serial device, it... this so-called baud rate actually... It's basically a frequency that generates data Right? Now, physically speaking, in reality, in actual physical terms,

we say this serial port transceiver, you can think of it as a serial port device. So this serial port transceiver, on our board, right? On this SoC board of ours, and this SoC, of course, has some CPUs inside it, and things like that,

which we won't worry about. The CPU. And this thing, actually counts as its peripheral device. There are many peripheral devices, and we'll just dot dot dot. Let's not talk about that. So the UART device, it actually belongs to the peripheral devices.

Now this peripheral, well, right now it has to go through one of these over here. Say we have a TX port, and this data needs to be sent out. Now, for sending it out, there is a frequency at which the data is generated, and we have to configure that.

So as for this frequency, how is it actually produced? It actually comes from outside, on our side we generally have some uh, a thing called a crystal oscillator. A crystal oscillator means that It is a component. It generates this fixed frequency.

So this crystal oscillator, or clock, it produces an input clock. Now, under normal circumstances, as we know, this clock on the board, it is generally a fixed frequency. For example, it is the fixed frequency X. Now here, the frequency Y that it produces,

is actually configurable. We have things like 115200, 9600, these frequencies, right? So this thing, on this serial port device, there are some components inside it, that can take this X as the input as the input. And this is essentially the output frequency.

By configuring some of the parameters inside it, we can make it produce— that is to say, we only have one unified input frequency x, but we can generate different output frequencies y. And that is something we can configure. Now, this configuration,

we need to make use of— the configuration on this component here, is set up like this: that is to say, on in our manual here, in this manual, you can scroll down, in this part of the manual, there is a table here, there is a table here

It's called the baud rate generator programming table. Baud rate generator programming table. Generate this programming table. What it actually means is, it describes it this way. You can take a quick look at it. The text is a bit small.

Let me zoom in a little. On this table of his, what concept is he expressing? That is to say, this, he says all the boards, typically will have, will have a 18432 MHz crystal, which is a crystal oscillator, which is what I just mentioned.

It is a a component it will generate a it has a fixed input frequency So, if we take this that is to say, when it's said like this, this kind of thing, it basically produces two kinds of there are two input frequencies that is, either a 1.8432MHz clock,

or else 7-point this higher one, just these two kinds it currently has two input clocks which is what we call X, the input clock frequency Then through some special register configuration, we can take this output, and configure it for different output frequencies,

that is, the baud rate. There are 50 and 75 You see, there are a series of things here. So we just need to take some special values in this register, and set it to these values, and it's equivalent to saying, it works like y equals f of x,

where this x, as we just said, is fixed at 18342 or these two values. Then we can adjust the parameters of this function f, which is basically adjusting the register, so that the resulting y, which is the baud rate, changes differently,

right at this point. Now, this table, has a correspondence relationship. How do we adjust it? Basically, this f, let's say it follows a linear rule like ax plus b, something like x plus b. We assume it works like this. So this thing,

this parameter A, we can adjust this parameter here. That's essentially what it means. So when we're writing this program, what our comment is trying to say is, what we want to produce, what we actually use is, we actually use 18432 as the input frequency.

Then, if we want to produce an output baud rate of 38.4K, then all we really need to do is set the value in this register to 3, and that's it. That's the idea. So which two registers are these? We describe them in the code. Let's take a look together.

In this code, What's written here means we're actually configuring this DLL and DLM these two registers As for these two registers, we also saw them earlier in this manual Where was it? Let me take a look. It should be around here.

What it corresponds to is actually it's an abbreviation. So, for LLM, it corresponds to what's on this table. Let's take a look. As for this stuff, you don't need to memorize it. It's mainly obtained by looking things up in the manual.

For DL, it's just this abbreviation which is short for Device Latch List that is that's what it means what I mean is the value you need to configure is not the value we just configured we just calculated it as 3, right? right? the value we configured earlier was 3

so 3, when we represent it inside the computer that is, all the configurable values are actually a 16-bit number so 3, in the computer, it is actually a number like 0x0003 so all the configurable values are 16-bit right, a 16-bit number like this

and 16 bits, it needs two two bytes to represent Right? Now, one register. In this UART, this UART, I forgot to mention earlier, for these UART registers, each register is 8 bits wide. So this 0x0003 is actually stored in two registers.

These two registers, one is called dll, and the other is called dllm. That's what it means. Now, for dll, this l here, this l, stands for low. What does low mean? It means the low bits. That is, this number This 03 has to be put into

the register called DLL. And this 00, the high bits, we have to put them into the register called DLM. So if you look at our code here, we are basically writing values into these two registers, writing values in, right? The low bits get 0x03.

Let me explain what these two lines mean. So what does the next line mean? Why do we have to set one more register, the LCR register, before writing this data? This LCR register, this is because... Take a look here, I've written an explanation for you.

That is to say, our DLL and DLM, these two registers, they share their addresses with our receive and transmit... As for these two registers, the enable register, actually shares an address, shares an address. Now, since there is a multiplexing relationship,

we need to write a particular bit in the LCR register, that is, to make it select that one, because one register can't be used for both at the same time. It either does that thing we just talked about, or it does the job of enabling interrupts,

or it does that thing we just talked about, setting the baud rate. So for this function, you can only pick one of the two. We need to flip a switch, to select the function of this register, to pick it. So actually, the function of this register,

the function of this statement, is really done by controlling the LCR's bit 7. Set this bit 7 to 1. As for why the code is written this way, we won't go into it here. That's basic C language knowledge. Setting a particular bit,

setting bit 7 to 1. As for why it's bit 7, you can look that up in the manual. It's the LCR register, and its bit 7 is used for exactly this purpose. Once it's set like this, it effectively means that, these DL and DM, can now be used to configure the baud rate.

Just to mention this here, you can refer back to our earlier explanation, and look at it together. Then the next one, is about this part here, here it's setting that parity bit. and so on — this concept. Now, this, it involves the LCR register.

You can go and check the manual. In that manual, the different bits have different meanings. Each bit in here, why it is set that way, well, here I think I won't go into detail, because explaining all of that would take way too much time.

You'd better go through the manual, look at these bits in LCR, and see what each of them means. I've already listed them out here. Why we set it, what we set it to, the exact value we set it to, you can just check the manual and see for yourself.

So, that being the case, So our serial port is essentially initialized now. The effect of our initialization, you can look at what's written here — it basically says, our data bits use 8 bits, and the stop bit uses 1 bit, and then there is no parity check,

and so on. Once we finish going through this, we'll call it a day. This is the serial port initialization. Now, since what we want to do is print content out on that serial port, then actually, for this serial device of ours,

we're really only using its transmit function right now. It's like on the diagram from earlier, since we now want to print out Hello and so on, then once we've initialized the serial device on this side, after getting it all set up,

what we actually use it for is to send the content over, right? So actually, we are only using its transmit function. The receive function we are not using for now. So what we are using right now is the transmit function. Now, about this transmit function,

let's take a look at how to use it. We say that UART operates in full duplex, with transmit and receive as two independent directions for data transfer. Now, on this point, I want to bring up something for you. That is, for data transmission and reception,

in general, we have two ways of handling it. One is called the polling method, and the other is called the interrupt method. So what is the polling method? It actually means this. Suppose we want to send a piece of data. Inside this UART,

there is actually a register. This register... Inside it, if you want to put data in, this register is connected to its data lines and sends it out. So the content in this register, as we know, you put one byte in, it puts one byte in at a time,

you put one byte in, and it sends one out. Put one in, send one out. But this sending, as we know, takes time. Right, everyone? In other words, this... you can't just send into it whenever you like, because if that byte hasn't finished sending yet

and you put a second byte in, you'll overwrite the previous byte, and your data will get corrupted. So, to achieve this effect, there is a way, that is to say, we have the polling method or the interrupt method to know when to put the data in,

so that it can be sent out correctly. Now, what we call polling, means we can keep checking a register, to see whether this register is currently free. If it is free, we send the data out, right? If the register is not free, then we don't send it.

That's what polling is. It is implemented through a loop, constantly checking it. Then there is also the interrupt method, which means that when this register is free, we let this UART device... It will notify us through an interrupt.

It's like giving us a call to say, "I'm free now, you can put it in." At that point, we can use the interrupt approach. As for the interrupt, since we haven't covered that concept yet, we'll hold off on it for now. Let's focus on how this polling works.

Here, there are two registers involved. One is called the transmit holder, that is, the THR register. This register, as we call it, is the register for putting data. The data we want to send, we just need to put it into this register,

and it will send it out. So how do we know this register already finished being processed by it, already sent out. Then we use a register on this side called the line status register. There's a special bit in it that tells us whether the transmit register is already free.

So combining these two registers, we can implement data transmission. Let's look at how the code is actually written. The code is actually quite simple. You can see that we've implemented a function called uartputc, this function.

This function actually mimics the putc function in our C library. As you know, putc only sends one byte at a time, right? And here you can see there's just a while loop here, What is this while loop doing? What this while loop is actually doing is

As you can see, what it's really doing is continuously reading a register specifically this lsr register reading out the value inside this register and at the same time, through this AND operation, checking whether bit 5 of this register

is currently equal to 1 If it is 1, if at this point, that is, if it equals 0, then the value read out here is not yet 1 and if it's not 1, that means this register is currently not free that is, our THR register here is currently not free yet

and while it's not free, it just spins in this infinite loop So when does this code... If we can get down to this part here, that means the LSR we just read out, bit 5 of it, is 1. Think about it, right? So when it's 1, it means this register,

the THR here, is free. Being free, it goes down to the next step, and at this point, through a write operation, we write the character into this register. So you can actually think of this line as meaning, the data has been sent out to the window,

thus achieving synchronization with this sending device. this synchronization effect We call this approach the polling method. The polling method. So after implementing the sending of a single character, well, you know, when sending,

say I want to send a hello or something, what we actually do is break it down into each character, and call putc once for every character. and just send it out See what I mean? And that's how the character gets transmitted. So using the serial port,

we can achieve this effect. As for the related code, you can go take a look. It's the example we just talked about, in that code's os, the example inside hello rbos. Feel free to check it out. We'll leave this one to you, and in the second class later,

we'll spend some time going through it together. You can try this example out as an exercise, and see what we just covered, combining it with what we just talked about. Go and take a look. Alright, let's, let's continue. Now that I've gone over the code with you,

I forgot to give you a quick demo So let's take a look at that simple Hello demo from just now After we compile it once we just run make run, we can see that a Hello appears on the screen RBOS Now, when it prints this out here,

what we need to understand is that we are actually controlling the UART device inside the emulator and you can imagine that there is a virtual UART line and through that UART line from the device, it gets printed to our host machine's console

and then these characters appear on the console and those characters are what we printed out And the corresponding code is in our hello, our kono.c code, this function that we talked about earlier In other words, it's actually jumped into from assembly

the first C function, right? Now, in this C function, right at the start, it runs a UART initialization, getting that serial port device initialized. Then, through a puts, this puts, inside it actually, for each of its characters,

calls that putc, and then takes each character and, through the serial port, prints it out to our console. And after the printing is done, of course we just write an infinite loop, and it just stops there. So as we can see, in our output here,

it doesn't go any further at this point. So that's basically the effect. Let me briefly explain it to you. Alright. As for this simple piece of code, you can just go back and look through it yourselves. I won't spend class time

going through it here in the lecture.
