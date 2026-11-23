# English Narration Script

All right, good morning everyone. In the last class, we mainly talked about some concepts on computer organization principles, right?

So today we're moving on to Chapter 2, because as we know, the goal of this course is to develop an operating system. Now, at the end of last class we talked about, if you want to develop an operating system, you first need to have some understanding of the interface to the underlying hardware.

And the interface between the operating system and the underlying hardware, we call it what? It's called IC, that is, the instruction set, right? So today we're going to give you an introduction to some related concepts about instruction sets.

This class has two main parts. First, I'll introduce to you roughly what the concept of IC, the instruction set, is. And the second part, because this course targets a specific instruction set called RISC-V. So we will also give you an introduction

to the RISC-V instruction set. That's how it is. Now, this is ours. Starting from this chapter, we'll have some reference manuals. the referenced material you should take a look after class, that is and it will also be noted in the slides

that is, some content or figures referenced which sources, Reference 1, Reference 2 they'll be listed right here among them, Reference 1 and Reference 2 are actually the RISC-V privileged architecture standard specification this is in our

I don't know if after you go back that is, did you download the thing on Yun and above? that under reference in the references there are these two should I go over this for you? let me point it out under this REFS, which is reference

Right below here, there's this thing. The first two are called Reference 1 and Reference 2. These actually refer to these two documents. This one and this one. Hold on a second. This one and, I believe, this one. These two. Then there's one more.

There's one more called Reference 3. Reference 3 is the RISC-V manual. This should be it. It should be this one. It's this one. This one is in Chinese. The other two are in English. As for this third one, the Chinese one, you can actually use it as

an introductory manual for learning the RISC-V instruction set. It was translated by people here in China. The original was written in English and has been translated into Chinese. Relatively speaking, it might be more accessible to students in China,

so you can start with this document to get a general sense of the whole picture first. Then the first two are the standard specifications, and they're entirely in English. During the course, we'll go through this reference document together,

and we'll also refer back to these materials often later on. So let's start with the first part, which is a basic introduction to the ISA. It mainly covers a few sections. First, an introduction to exactly what RISC-V is. Second, why we want to design RISC-V.

And then it introduces some related concepts about the RISC-V instruction set, such as the classification of instruction sets, instruction set width, and also some things about instruction sets — that is, we have many kinds of instruction sets today.

Now let me also introduce some of the more well-known instruction sets to you. So first, let's look at exactly what RISC-V is. Well, RISC-V is actually an English abbreviation. In this course of ours, we'll run into a lot of English abbreviations.

So in the lecture slides, we've spelled out these English abbreviations for you in detail as well. You should still get familiar with them, because when you're... since this kind of, how should I put it, I mean, material on computer systems

is mostly in English. And in English, these abbreviations are used very heavily. Plus, once you understand what the abbreviation stands for, you'll grasp the concept itself better. So I do recommend that you... So try to memorize the original English term.

And that will also help everyone improve their English a bit. Just a little boost. So what is ISA? In Chinese we translate it as 指令集架构, or instruction set architecture. It's the abbreviation of the English phrase "instruction set architecture".

Right? Once you see that, you'll remember it. And the concept of ISA becomes very easy to understand. So, when we talk about the instruction set architecture, what exactly is it? Well, right here, I've written it in bold for you,

just to emphasize the point. That is to say, it is not... it is not some simple... in other words, what is it? Some of you might treat the instruction set architecture, that is, well, Because you've all learned some assembly and such,

you might simply understand it as just some, like, assembly language, or something used for programming. But in reality, it's far more than that. I should say that we call it... its fundamental concept is that it is a specification of an interface.

Now, since it's a set of interface specifications, it is actually a standard. You see what I mean? Don't confuse it with a kind of assembly language and mix the two up. So on this, we can actually see it from the figure on the right.

You can really tell from it, and in fact this figure was mentioned in the last class too. That is, which level RISC-V sits at. We've talked about this diagram before. The top part is actually the software part, and the bottom is the hardware part,

right? Now, once the software part on top is layered, the very top layer is the application layer, meaning the applications we write, this concept. And in the middle is the operating system. Now, between the operating system and the application layer,

there is also an interface, right? There is also a set of specifications. What is that set of interfaces called? Someone mentioned it last class: it's called system calls, right? Now, system calls, as we know, should really also be a standard,

not just a simple set of functions. Why is that? Because you've all heard of the concept of POSIX, right? POSIX. POSIX is a set of standards. You get that, right? It's just that its form manifests as a set of C-language function calls.

But actually, if you go look at the POSIX standard, it's really a body of text, a book. It spells out in there this is what you should do, and this, and this. That's what it means. So RSA is something like that. It's also a set of standards,

not just simply telling you some assembly language instructions. It's not limited to just those things. So beneath this RISC-V is what we call the hardware. Now, as for the hardware, on this diagram here we have already given you a hint about this hardware.

Actually, in the industry, we call it Microarchitecture. So, from now on, if you get involved in some low-level development, this microarchitecture is what we often refer to as the concept of microarchitecture. So what is microarchitecture?

As for microarchitecture, you can take an example. That is to say, the one you may have used a lot before, is probably something like x86, that instruction set architecture, right? So we know that for the x86 instruction set architecture,

it's it's just a standard, and someone actually has to implement it, right? And the one who implements it, is actually not necessarily only Intel. Are you familiar with this concept? I mean, for this x86 instruction set architecture,

apart from Intel implementing it, I mean, apart from Intel's chips implementing it, what other company also does x86? A fairly well-known company. There's a company called AMD, right? So both it and Intel implement the x86 instruction set architecture,

but the specific implementation underneath is different, right? And their specific implementation is a concept called microarchitecture. it's something tied to the hardware everyone follow me there now, as for microarchitecture,

what is it actually concerned with? you can think about what it cares about, since it's tied to the hardware, then it must be related to the manufacturing side of things, right? so what does it care about? for example, this chip,

if you want to build this chip, if you want to implement this interface, it's just like our software, if software wants to implement the POSIX interface, we have plenty of C library functions, right? and those C library functions could be made by this vendor,

or by that vendor. Right now we have many kinds of C libraries, there's the GNU C library, and there are other C libraries as well. Now, the instruction set architecture is like this too The microarchitecture is hardware It's one implementation of the instruction set architecture

And what it mainly cares about is For example, things like power consumption per unit area For example, how the execution is actually implemented Everyone builds the same set of X86 CPUs Why might Intel's be a bit faster While the other one is slower

Why does one generate more heat While the other generates less And the thing they care about most In the end, what is it It's the issue of cost Think about it, isn't that right So These are the things the microarchitecture cares about

Of course For us as software programmers We may not care that much the reality of this microarchitecture, what we mainly care about is still this set of interfaces. So once we have this set of IAC interfaces, we no longer have to deal with

this microarchitecture underneath —the specific microarchitecture. You see what I mean, right? Our world, or at least the world of computers, is layered just like this. It's wrapped up layer by layer. So everyone just does their job at each layer.

So when we say this microprice —not microprice— meaning the instruction set, what exactly does it define? Let's just list it briefly here. Later on, when we get to the details, we'll go through them one by one. Let me briefly explain here.

For example, it defines some basic data types. For instance, this byte. Or what a word is. These names actually come from the microarchitecture, and they're defined for you from the instruction set perspective. For example, one byte is 8 bits,

right? How much is one word? It's 32 bits, correct? A halfword. We'll run into this concept a lot later on. A halfword is 16 bits. They're all multiples of 8. What else is there? What else does the ISA define? Registers. Because as we said,

when we interact with the hardware to operate it, When it comes to the CPU, it is mainly about operating its registers. For example, this given architecture, that is, on this CPU, how many kinds of registers it implements, those have also been standardized.

Then what each register is for, and so on. Then there is a third thing, which is what everyone cares about most, and that is instructions. Because when we write assembly, it is mainly programming, and how each of its statements is written out.

That is really the concept of instructions. Then there are addressing modes, and how exceptions and interrupts are handled. and so on and so forth Let me just list a few things here. So the main thing we want to... This is mainly to get a better feel for...

That is, what is... What I mean is, an instruction set architecture... is a standard. It defines a lot of things... rather than just being simply... an assembly language. So everyone... This slide is mainly to emphasize this point to you.

Now, as we said... why do we design this instruction set architecture? Actually, we mentioned it just now. It's the concept of layering. Think about it, isn't that right? Once we have this thing... we can say that for the upper-layer software...

so this upper-layer software... It actually refers to writing an operating system, this kind of low-level stuff like a bootloader, the developers involved in this kind of work. They also do software, so they don't need to care about

the underlying circuit structure. And that circuit structure is what we just talked about, the concept of microarchitecture. We actually just interact with the underlying layer through a standard set of interfaces, and that's all we need to do.

Now, on this diagram, let me point something out to you, which is that we should also understand the development history of this RSA instruction set architecture. That is, the first to propose this instruction set architecture

was who? It was the company IBM. Do you all know the full name of IBM? that is, International Business Machines it was actually originally a hardware company it made machines you can tell just from the name it made computers so back when it was doing this

probably in the last century which decade was that again I don't know, the 60s or maybe the 70s so when it was developing this kind of computer being a pioneer, it was like this before there were instruction set architectures as everyone knows, for every model of computer

its processor its operating interface was different say this computer there were, for example, 10 computers That company had 20 computers and each computer served a different purpose. So things were pretty chaotic. Now, for IBM,

by that time they had already grown to the point where their company's products had reached three or four models, and they found this problem was getting more and more serious. Think about it, because of this— since the underlying interfaces were different,

they had to have four teams, at least four teams. The software teams had to develop different compilers, operating systems, and so on. So the cost was just too high for them. So starting with the IBM 360 machine— this machine, which is this one in this picture—

let's take a look at what early computers looked like. This is the machine and it really doesn't look much like our computers today. Do you all see that? There are so many buttons on it. You can think of these buttons as, how should I put it,

this thing is actually the early concept of the console. You know, our computers today have what's called a Console, that console, the one where you're always typing away at the keyboard, that input interface. Don't we call that the console?

Well, the early console was really just this thing. It had a lot of buttons on it, for controlling the computer, for making it do things, switches and so on. So this machine is called the 360. So IBM, starting from this machine,

defined the first instruction set architecture. And what it defined, is basically what we just talked about: what registers there are, these instructions, and also the concept of a byte. So in all the computing we've done so far,

this concept of a byte actually started with this computer. Before that, everyone's standards were kind of all over the place. So starting from this computer, we defined it: one byte is eight bits. This standard was set right there,

and we've been using it ever since. That's how it is. That's how history played out. So it created a kind of abstraction between hardware and software. So after RISC came along, we're still in that situation today. Alright, so now that we understand what an instruction set architecture is,

and why instruction set architectures were invented in the first place, let's look at some concepts related to instruction set architectures. Because they'll come up from time to time later on, so we should get familiar with them.

So mainly what I want to talk about here is the classification of instruction set architectures. Typically, this is actually covered in many textbooks as well, the typical split is into two types: one is called CISC, and the other is called RISC.

So CISC, its full name is Complex Instruction Set Computing. In other words, the complex instruction set. Now, RISC stands for Reduced Instruction Set Computing, which is the reduced instruction set. Now, the one we'll talk about later,

that RISC-5, you can tell just from the name which category our RISC-5 belongs to. It falls under this classic instruction set category. Now, the main difference between the two, where does it lie? It's written up here too. With the complex instruction set,

you can think of it this way: its instructions, its... you can understand it like this: in order to implement, Take a computer, for example, it has many functions, right? And for each of these functions, you design a dedicated instruction

to go with it. Now, as you can imagine, as computers develop, and their functions become more and more complex, a certain trend is bound to emerge. That is, when the CPU is implemented based on a complex instruction set, built around this concept,

the number of instructions it supports grows, and keeps getting larger and larger, more and more bloated, right? Now, a classic instruction set takes a different approach. The idea is to use a few— to define a set of common, basic instructions,

and then for more complex functions, you use those basic instructions— combining them through addition, subtraction, multiplication, division Like these basic instructions, you put them together. So this way, it keeps the instructions... that is, the number of instructions,

the number of them, the variety, from growing too fast. Now, some of you might ask, why does this happen? This is also for historical reasons, because back in the early days, actually the first ones to come out were the complex instruction set computers.

They came out early on. So why is that? It's mainly because, as you know, in that era, inside a computer, what was the most precious resource? Actually, even looking at it now, In fact, this phenomenon still exists, that is, its memory is extremely precious,

because back then, given the manufacturing process and cost issues, a computer, like the one we just saw, inside such a complex computer, its memory could be pitifully small. That was caused by the conditions at the time. So, since memory was scarce,

then, as you know, when writing programs, what did they have to do? They had to be extremely frugal. Right? If I want to implement a very complex function, the program I write to generate this, it has to be as short as possible.

Because the storage, probably, even on a mainframe like this, it was probably only a few tens of... I'm not sure if it ever reached the megabyte level it was pitifully small We might not really feel this nowadays, but back in the day, even a single byte

had to be saved. So think about it — given that starting point, they wanted the instructions that is, the code you write, the machine instructions generated from it, to be as short as possible, so every single instruction had to be fully utilized

to accomplish a specific function. Think about it — isn't that the case? So naturally, this phenomenon emerged. And later on, as our technology advanced, as we know, memory and everything became cheap. Now, as for the size of this program,

we actually don't care that much anymore. What we care about more is what? What we care about more is its, for us programmers, a kind of comprehensibility, readability, including that, this compiler, its, since the compiler is also getting more and more complex,

then we can actually design this instruction to be very simple, and let the compiler help us assemble these complex functions. Don't you think so? This, or let's say, let me give you an example from our daily life. Actually, this might make it easier for everyone to understand.

You all know, for example, take Chinese characters. You've all studied Chinese in school, right? In the early days of Chinese writing, classical Chinese versus modern vernacular Chinese, what's the relationship there? Well, it's actually kind of similar to,

the idea of complex instruction sets versus, simple instruction sets. With the classical Chinese of the past, as you know, a single Chinese character had to convey, a really complex meaning. Think about it, isn't that right? Why?

Why did our Chinese, our ancestors, design our classical writing, to be like that? Because as everyone knows, in the early days, paper was too expensive. And it wasn't even paper — what was it? It was those bamboo strips, that kind of thing, right?

In the West, they didn't even have that. They wrote on parchment and so on. Only highly educated people could write. Common people simply had no chance to write at all. So since these bamboo strips were so complicated to produce,

every single character had to fully express a meaning. That is to say, each character packed in a particularly rich set of meanings. We used to talk about that saying, the one about 'sweating oxen crossing the ford' — or 'learning rich enough to fill five carts'.

Do you know how many books five carts could hold? Actually, those books didn't have that many characters. Do you all know that? In the past, educated people probably didn't read all that many books in their whole lives. But because every character carries so many meanings,

the amount of information conveyed was actually quite large. Then later on, with vernacular writing and cheaper paper, the cost of learning kept dropping, and that's how vernacular writing came about. Why use vernacular writing?

Simply because it's easier to understand. Think about it, isn't that right? So this classic instruction set, the trend now is to develop toward the classic instruction set. And there's a similar reason behind that, too. Of course, the two of them

are also converging with each other. So this one, if we look at it from a historical perspective, there is this classification, and that is the complex instruction set. the concept of a classic instruction set Now, the RISC-V we'll be learning later on,

should be a it's also a standard classic instruction set. As for complex instruction sets, early on, for example, Intel's processor, it was, from the very beginning, a complex instruction set. And now, it's also slowly shifting towards a classic instruction set.

So that's what we just talked about— the classification of instruction sets. Now the next one, is about this instruction set's concept of width. Because when we buy computers these days, we actually often care about this too, like, say, this notion of 32-bit

and 64-bit. Well, this actually has to do with the instruction set. And later, when we write code, we'll keep bringing up this concept too. So now we need to be clear on, what exactly does this width refer to? When we say this width, what does it actually mean?

It actually refers to the CPU— the general-purpose registers in the processor, their width. We know that a register, you can think of it as basically a storage unit, individual storage units, storage units inside the CPU. This concept—our concept of registers—

in the diagram from last class, go back and take another look at it. Where does it sit? What role does it play inside? Still remember? Think back to what we went through last class, the one I demonstrated to you. the execution process of a program

what role does that register play in it because as we said, when the CPU is actually processing data, it can't just directly take that data out of memory and start using it right away. It first has to take the data in memory and put it into the registers inside the CPU,

and only then can it work on it from the registers. Right? So registers are extremely important. Now, the size of this register determines the so-called width of the ISA instruction set, and it actually also determines the processing power of this CPU.

Now, a typical Now, as for the size of this register, we've listed it here: there are 8-bit, 16-bit, 32-bit, and 64-bit. What that actually refers to is, a 'bit' is just one binary digit. One binary digit, one bit, which is a 0 or a 1 — one bit.

Now over here, I've just laid out a simple list, meaning that one 8-bit value... and you'll often need to have this in your head, you need to be familiar with this kind of calculation, because in low-level development, you're very often dealing with individual bits,

calculating bit by bit, and encoding things. So these numbers, you should all get quite familiar with them going forward. 256, 6536 But that second number is too long so we won't memorize it. But these are all powers of 2. What do they look like in hexadecimal?

When you see these numbers, you should develop a kind of professional instinct. What do I mean by professional instinct? You'll gradually cultivate this sense over time. Then low-level development will probably become easier to get into.

Now, as for 8-bit CPUs, in the early days they were definitely all... There were even 4-bit ones, for example, from Intel, there was a chip called 4004. You can tell from the 4 at the beginning that it's 4-bit. As for the 8-bit ones,

they were actually the first to become more practical. So we say The classic 8-bit machine is what the 8051. As for the 8051, I don't know if you guys at school now still cover this chip in class. I think it should probably be phased out by now,

because it's just too old. Really too old. But back in our day, when we were learning microcontrollers, this chip was still around, and we actually used this one, because this chip was so famous, it was used incredibly widely, both internationally and,

in particular, here in China. So it's an 8-bit machine, meaning that inside it, each register is only 8 bits wide. It can only read 8 bits at a time. That much data into it. So you can imagine its processing power. Then later it became 16-bit.

With 16-bit, the more typical ones are the 8086, and also MSP, which are some microcontrollers from TI. So for something like the 8086, what we call x86, x86, actually started from the 8086 generation. 8086, 80286, 386, 486. That's where it comes from.

Now, this 8086, was the earliest one, and it was actually still 16-bit. Then later, moving on to the 286 and 386, 32-bit ones started to appear. So at that time, after we entered the 32-bit era, our computers and the like, that era started to arrive,

the PC started to come along. And now, we can say that the mainstream processors, have actually all transitioned to 64-bit. Now, as we know, when it comes to the kind of chips we buy these days, their 64-bit actually refers to the width of their general-purpose registers,

which is 64 bits. The range that a single 64-bit number can represent, that is quite astounding. Right now, the mainstream devices, including our phones I won't say anything about PCs And then there are servers Those are generally 64-bit

Now some of you might say, why don't we have 128-bit yet? Indeed, we don't Unfortunately Strictly speaking, there is no 128-bit general-purpose computer, no 128-bit general-purpose processor Not yet The reason is very simple It's just not practical yet

Because human needs haven't reached that level I remember reading an article, someone said that even if you take all the atoms on the entire Earth and add them all up, it's not much bigger than 128 bits the number of values it can represent isn't that much larger

what this really means is there's just no real possibility of doing computations with 128-bit data but then again, it's hard to say for sure maybe as technology advances 128-bit will come along at some point too for now, at least, let's not worry about any of that

later on, when we when we actually build the operating system, we'll choose a 32-bit system 64-bit the reason we choose a 32-bit system is really just to make the calculations easier for you when you do those calculations you see what I mean? if you had a 64-bit number

and I asked you to do the arithmetic on it perform some operations it might get a little tricky it's not that we can't do it but from a teaching standpoint, we've decided 32-bit is enough. Besides, 32-bit still has its place in today's industry.

For example, in embedded systems, it's still used extensively. So we went with 32-bit for you to practice with. That's the concept of instruction set width. Now at the same time, keep this in mind: often when students learn this concept,

they confuse two things. The width of the instruction set and the length of each instruction are two different concepts. For instance, some students say, when they hear the CPU is 32-bit, or the CPU is 64-bit, does that mean each instruction is 32 or 64

That's not necessarily the case. For example, the RISC 5 instruction set that we're going to learn, whether it's a 32-bit processor or a 64-bit processor, the instruction encoding, each instruction is 32 bits. You really need to keep that in mind.

So don't get that mixed up. That's the concept of the width of the instruction set. Now, moving on, let's take a look at let me see here, let's take a look at what famous instruction sets there are. Now, I don't know if any of you have come across these before.

Which ones have you encountered? When you learned assembly at your school, which one did you learn? What was it? Is it x86? It should be. Because x86, in our country, is used relatively more. So, we won't say much more about x86.

It actually just is I even got some icons here. How nice they look. So this is x86, right. Let's play a matching game. Can everyone connect them? Which one is x86? Is it this one? It's definitely this one. Intel Inside. That advertisement is plastered everywhere.

It has squeezed out our entire domestic market. Every bit of it. So, we won't say much more about x86. Have you all heard of this, SPARC? Does anyone know who it connected to? Speak a bit louder, can I hear you? Sun, is that right?

Good, you all still remember. That's not easy. This Sun... Sun, actually, in the history of our computing development... it was once a... played a very pivotal role. It was a... wasn't it the dominant leader? I don't know if it can really count as the dominant leader.

Back in its day, it was actually very powerful in the market. And its... founder, I think, was also a university professor. I remember. After he built this thing, he went and founded a company himself. He left the university. And so he founded this...

But, unfortunately... This company, like all of you are probably still using Java and such, it actually did not just do hardware, it did software too, including operating systems. It had an operating system called Solaris. And the Java language that we use,

was actually invented by this company too. Unfortunately, it ran into some commercial problems, and eventually, as we say, the company couldn't hold on anymore. So which company ended up swallowing it up? You probably know, it's Oracle, right?

Oracle swallowed it up. Now, after Oracle acquired this, unfortunately it didn't carry on with this, its CPU business, that is, the hardware side of the business. it was able to continue and carry that on but later it was terminated

so this this chip, in fact, is rarely seen nowadays but not that many years ago, actually, in many of our domestic large it was mainly pushed for some building servers and such, things like that, like some of our telecom equipment,

the servers inside banking equipment, it was used quite a lot there Now, which one is Power? Which company is it from? It's from IBM. Oh, I forgot to mention earlier, this X86 We say it's a classic complex instruction set architecture.

And as for the ones below, most of these are classic instruction set architectures too. These are all classic instruction sets, that is, reduced instruction set architectures. Now, Power is from IBM, and it's still used in these 64-bit systems.

That's because IBM, well, it actually does everything from hardware to software. Its products are still in use today. It's just that we don't usually come into contact with them much, because they're mainly used in large enterprises,

like banking and power companies, running on those big servers. Now, ARM goes without saying, because we should all be fairly familiar with it. And as for ARM as a company, it actually came along quite late, but you could say that

its market positioning has been quite accurate. It was mainly aimed at that point in time that is, at handheld devices and mobile computing which is really our mobile phones At that time, in the course of mobile phone development,

it seized this opportunity while other companies did not seize it. The most classic example is a company like Intel, Intel simply failed to seize this mobile phone opportunity in the development of mobile devices. It's not that they didn't see it—

they did see it, because everyone is smart, and they all knew they should do it. But why didn't Intel do it? The reason is that they had done so well in PCs and servers. As we know, when they when you've done something so well,

asking you to immediately change course when you go do an unfamiliar line of business, there's actually a psychological resistance to it. You all understand what I mean by that, right? So, that being said, but ARM, it had no baggage.

The moment it got started, it jumped straight into doing this. Because if it didn't do this, it would be dead. You see what I mean. And as for Intel, it had that... that business was just too profitable. Back then, PCs, servers and all that,

it was making way too much money. So it looked down on that business. It just didn't care for it. And as a result, ARM rose right up. so this is kind of a happy coincidence there is no deep reason behind it so what about ARM? well, riding on

the spring breeze of mobile computing, it took off all at once and now our small devices, lightweight devices, mobile devices of this kind, mobile phones are basically all ARM ARM chips so its main characteristic is that its chips,

because it uses a reduced instruction set architecture, its power consumption and everything is very low, and furthermore, what it adopts is quite different from other companies It's not like that. ARM, as we know, it doesn't make chips.

What does it actually do? It sells something like intellectual property, this kind of IP concept. Right? They design the stuff, provide the solution, and then sell that solution to others, and others build with it. That's actually how it works.

Take the chips Huawei makes, for example. They've really just bought ARM's solution. But Huawei, it does make chips, but in reality, Huawei, to make chips, uses the instruction set architecture It's still ARM's intellectual property.

So, another one is MIPS. MIPS is this one. This MIPS, this is, let me see which American university this was from. I've forgotten all of a sudden. He was also at a university. Was it in California? Or which university was it? A professor at an American university

invented this whole thing. Then later he started a business, founded a company, and created MIPS. Now, MIPS, its history has been quite a winding road. Actually, in its early days it was technologically very advanced. It used a reduced instruction set architecture.

So it went into a lot of our embedded devices including especially chips used in routers, where it was really common But later on, for market reasons as well, it just didn't continue and after that it kept getting acquired and bought up by other companies

until in the end it just couldn't go on And now, MIPS, the company, there's a piece of recent news saying that it's also preparing to move over to RISC-V, so that's how it turned out So it's quite a pity, really But actually, MIPS,

this instruction set architecture, in the course of our computer's development, still played a very significant role In particular, I mean, For example, some of our classic early classic computer science textbooks, because it was invented by a university professor,

in the textbooks, actually many textbooks use MIPS to teach, including what we're going to cover later, the RISC-V instruction set architecture, it actually borrowed many concepts from MIPS. So these two, MIPS and RISC-V, these two things,

they actually share quite a history. Quite a history. Which university was it? Let me think, I can't recall it right now. I think it was at it was that... I actually forgot it. It's Stanford University. There's a person at Stanford University called

The Chinese translation is Have you heard of it? What seems more familiar is a famous wine, right? Actually, you may also know this name, which is Xuan, is it this Xuan? Xuannisi, have you heard this name? A luxury brand, a luxury brand,

but actually it's a foreign person's surname, I think, Is this the surname or the given name? So this professor is actually called Xuenishi, and he invented this thing at Stanford University. Alright then, That covers most of the better-known ones.

Of course, there are some others, but since our time is limited, I won't go on about them here. So mainly, this course is about learning RISC-V. RISC-V is a new instruction set architecture that has been rising recently. As for the content related to this instruction set architecture,

I'll stop the brief introduction right here. How about we take a break first? Because last time it seemed like the lecture went on too long. Let's try to keep each lecture to around 40 minutes. Let's take a break first.
