# English Narration Script

Next, let's talk about the thing you care most about: a brief history of the RISC-V instruction set architecture, along with its current state of development and key features. And then finally, we'll take this instruction set architecture

and cover some of its basic concepts, which are mainly just our things from that spec, some of the points in the standard that are more important, I'll briefly go over them with you first. As for the details later, in, in later courses,

I will bring them up to you again. Now, when we talk about this instruction set architecture, it's fair to say it has been developed for many years, right, many years. Now, with so many instruction set architectures out there today,

we should say they are actually all, they should all be said to be the intellectual property of these companies, rather than being open. So, so as you know, we're now at this point where, from the software side of things, in our software industry,

in our software industry, these days this kind of idea of free and open source, has now become more and more intense, more and more intense, wouldn't you say? Now, this, for example, if we look at the software industry, this open source concept,

what kind of state is this development in? Take operating systems, for example, let's break it down along a few dimensions, one is: are the standards open? and another is: is the implementation open source? So what are the necessary ones that correspond to this

Let's take a look. For example, take the operating system. As for the operating system, we know that for standards like this, there are open ones, all of them POSIX, they're all open standards. Right? And in terms of implementations,

things like Linux and FreeBSD, those go without saying. Then what about closed source? Well, of course, Windows is the representative. Now, with Windows, we know, Microsoft is also embracing open source now. Right? Then, as for compilers,

for instance, the standards, which are really the languages, the programming languages. So, C, on the compiler side, GCC, and these LLVM... LLVM, I don't know if you all have any classmates who've looked into it. I mean, you all probably use GCC quite a bit,

right? Of course, our course also focuses mainly on GCC, but when it comes to the LLVM compiler, it has this momentum of coming from behind. Perhaps in the near future, it's quite likely that it will overtake GCC. But that's hard to say.

Right now, the development abroad, including in the industry, is still very rapid. Now, all of these are open source. As for this open source... on my side, what I mainly look at is that there are these specific manufacturers,

for example, Intel. compilers that they developed themselves. Actually, if any of you have been exposed to this, if you've used some of these Intel or ARM IDE development tools, the editing underneath is actually built on these.

Then databases, and so on and so forth. That goes without saying. Now, what we mainly want to point out here, is that when it comes to instruction set architectures, what is the state of open source at the hardware level? What does that actually look like?

Well, before RISC-V and these things came along, instruction sets like X86, and ARM, were actually not open. X86 goes without saying, because only Intel and AMD make X86 chips. As for ARM, they grant you a license, but you can't just modify it however you want.

Everyone get that? Take Huawei for example. It bought that instruction set license, and after buying it, what you can actually do on top of it is fairly limited. ARM will stipulate that you cannot touch these things. So, mainly because these things

are not completely open and free, how should instruction set architectures develop? They will actually move toward freedom and openness. Well, RISC-V starts from exactly this idea and introduces an instruction set architecture.

Now let's take a look at the historical development of RISC-V. I've actually pronounced RISC-V quite a few times already, and now you all need to know how to say it too. It's 'RISC-V' — don't spell it out letter by letter every time,

that would just be too nerdy. It actually has its own pronunciation. On its official website, in the standard, it explicitly states that this is how it should be pronounced. Since it's a standard, everyone should just stick to the unified pronunciation.

It's called risk, that is, risk five. But it doesn't mean that risk. We know risk means adventure in English, right? It's just how it's pronounced. You say risk five. Now, since this is the V, we know it represents the concept of 5 in Roman numerals,

right? So this five really does mean five. So actually, it really is, this standardized architecture, It is actually considered to have developed into the fifth generation. But it wasn't until the fifth generation that it was released publicly,

as an open-source international standard. So it took this 'Five' and made it part of the name of this instruction set architecture. That's how it is. So where did this instruction set architecture come from? Well, as you can see,

it was developed inside a university again. From this, we can also roughly see why the basic science in the United States, as it develops, we say is quite impressive, is mainly because their education is quite strong. Have you noticed?

These more famous instruction set architectures, the instruction set architectures we talked about earlier, many of them came out of universities. For example, What's this called? That one. A few of the ones I just mentioned in here,

for example, this one, this SPARC, and then that MIPS, RISC. These are actually all just research results from university professors, and then turned into products. Instruction architectures like Intel's, even though they're very popular now,

back in the day, among these, if you look at it from within the industry, it was actually considered a pretty low-end architecture. It's just that its market success was better. You all get what I mean, right? But technically speaking,

it's really still these things that came out of academia. Its design is a bit more elegant and refined. Now, this project actually originated from the University of California, Berkeley. That school, as you know, is also very famous.

Earlier we mentioned Stanford University. These are all classic top-tier universities in the United States, and they are also the schools that are particularly strong in computer science and the computing industry in the US.

So this, it actually started back in 2010, at their university, when some faculty in the computer science school, they wanted to offer some foundational courses, for example, courses like compiler construction, computer organization, and so on.

So, as we all know, when it comes to this course, just lecturing on theory is actually very dry. That's something we also have to overcome, meaning there definitely has to be a hands-on component. And the instructors here are quite impressive.

They didn't just grab something off the shelf, because of course they did their research first. They wanted to use some of the CPUs that are popular on the market, to teach the students. So at the time, they did some research,

listing the resources, for example, like x86, ARM, but they found that each had some problems. What problems, you ask? Take x86, by the year 2010, it had already become very complex, because it has a huge number of instructions.

As we know, What kind of instruction set architecture is x86? It's a complex instruction set architecture, a classic example of one. So it has a huge number of instructions, over 1000 instructions. Now think about how we'd teach that in class.

There's just no way, right? Cover too little, and it's not enough. Cover too much, and the students simply can't finish it all in a single semester. So there's no way we can use this thing. Of course, there's also the IP issue,

that is, the intellectual property issue, because some of it might require paying fees and so on. Now, ARM has the same problem, a similar one. And as you know, with ARM, back in 2010, there was another very serious problem: it still didn't support 64-bit.

Actually, as everyone knows, the ARM chip is so popular now, but it's really something that came along later. Like around 2010, which really wasn't that many years ago, before that, about 10 years ago, it only supported 32-bit.

Because back then, for phones and such, 32-bit was enough. But of course now 64-bit has come along, so these popular ones on the market, these existing architectures, just don't work. That's a real headache. So these professors are obviously pretty awesome.

They said, if you can't use them, what do you do? Just build one yourself. That's where they really show their backbone. Now, of course, their development didn't just happen like, they decided to do it and it was done. Actually, since they're called RISC-V,

that tells you they actually started early on, and had already laid the groundwork for many years. You see what I mean, right? All of this foundation, especially this kind of foundation, this kind of foundational industry, is definitely not something you can

just throw money at and build overnight. This thing, at UC Berkeley, they were actually working on this as early as the 1980s, the early 1980s, around 1981 or so. They were already envisioning this kind of Of course, it was very simple back then.

Gen one, gen two, gen three, gen four, and it kept evolving until in 2011, they had developed the fifth generation. They felt it was pure enough it could be used for teaching purposes, it could be presented to the world. So he only did it

— of course, as he tells it, it only took him three months, basically one summer vacation. A few teachers from their teaching and research office, along with their students, managed to build this thing. But in reality, as we say,

it had actually been developed over many years. It is the crystallization of the knowledge of those who came before. So, that's why it's called RISC-V, and that's where the name comes from. As for its key people, these are of course some of the big names of RISC-V.

If any of you are interested later on, in getting involved in the community, they are still very active in it. So, one of them has what looks like an East Asian face, but I have a feeling he might be Korean. I don't know the details, I don't know where he's from.

I never looked into it. But this person is definitely Asian. The rest are all professors from these universities. Now, their roles have also changed somewhat. As we know, RISC-V later established a foundation. If you're interested, you can go to the official RISC-V website and take a look.

This foundation, this foundation, the reason we say it can stay open and free, the key reason really is that it is not held in the hands of a single company. You see what I mean, right? Once it's in the hands of a single company or enterprise,

as we know, it becomes a matter of money. And to avoid that problem, They actually set up a non-profit foundation called the RISC-V Foundation. This is the foundation's website, so if you're interested, you can take a look. What they did was

essentially they contributed all of their intellectual property to the foundation. So it's all open source, free for everyone to use and to modify. And uh, there are no license fees at all. You can basically do whatever you want with it.

Now, uh, as we just said, it's a set of standards, so it's not turned into a product. You see what I mean? It's not a foundation that makes chips. It only maintains this set of standards. This standard is released publicly, so all the vendors,

including individual developers like you, can build on this standard to develop your own chips. So that is RISC-V's appeal. And the instruction architecture it has designed, you could say it targets all kinds of application scenarios,

including microcontrollers, that is, down to the tiny chips in your wearable devices, or supercomputers— its design has actually taken all of these into account. So its prospects are very broad. Over here on the right, I've excerpted

the definition of RISC-V from the RISC-V Foundation's official website. Actually, everything I just said, it's pretty much the same idea. You can go read the original text yourselves, and get a sense of this foundation. Why did they set up this foundation?

This explains what this whole RISC-V thing is, this concept. So this RISC-V... It came out of the university back in 2010. It has been developing up to now, so it's been about ten years or so. Right, ten years. And over these ten years,

with the establishment of the foundation, including many companies that have also gradually started shifting toward this this, this standard, gradually investing in it. So this whole RISC-V ecosystem, its growth has been very rapid.

Now, this is also excerpted from some consulting firms online, their market analysis of it. As you can see, the market development is still quite attractive. It says that by 2025, they say this market will consume... 62.4 billion CPU cores

Why is this number so huge? Well, you can probably understand it mainly comes from the consumer market side of things, where the volume is just enormous. Take your little phone, a small router, some tiny device, even a little pair of earphones at home,

and inside there's a small chip. You see what I mean? So the shipment volume is absolutely massive. That's why he said there will be 62.4 billion of them in 2025. Now, some of you might say, Wow, 62.4 billion? That's so many. That feels almost impossible.

In fact, this number, for the consumer market, It's really not that big a deal So this market segmentation is used in many industries You can see that its growth is still very rapid So participating in this, after this foundation was established,

having just a foundation alone really doesn't amount to much What matters mainly is the ecosystem That is to say, worldwide there need to be actual manufacturers and all kinds of players getting involved Now, through everyone's efforts,

the membership of this foundation has grown This is a relatively recent set of figures: it already exceeds 700 across 50 countries worldwide And as for China, its participation is actually very strong We know that our country places a lot of importance on this chip industry,

on this whole thing, and ever since some unpleasant things happened, our country has been putting a lot of investment into these choke-point technologies, a very large amount of investment. And chips are one of them. Now, can we just go and buy,

that is, buy something like ARM? We know that actually there would be a lot of problems, not just commercial problems, but also that the government of that country would actually have control over it. But with RISC-V, it has one advantage,

which is that it is owned by a foundation, and as you know, that foundation seems to have moved its headquarters to a neutral country, right? Actually, that's precisely to avoid this kind of problem, that is, to avoid being controlled by a company or a government.

So in our view, That is to say, if we can get involved and help grow it into something big, then this kind of chokepoint risk in the future I can't say it would be completely gone, but at least it would be much smaller. The key is that you have to be able to get involved.

You have to be able to — I mean, it's not just about taking part; you have to take part in particular by getting involved in setting these kinds of standards, and so on. Only then can you have — that is, only then can you control it, right?

So this, its development status is roughly like this. You can all get familiar with it. Now, next, everything before this was just some introduction, just a bit of introduction, so you know what it's all about. What RISC-V is like.

So next, let's take a closer look at some things about this instruction set. First, let me highlight its characteristics for you. So when we talk about the features of RISC-V, this is really also a summary of what came before, and it also sets the stage for what comes next.

They can be boiled down to a few points. First, it's extremely simple. Why do we say the instruction set is designed to be very simple? This was one of the original design goals. Because as you know, RISC-V came along relatively late,

so in its overall design, it actually drew on a lot of the design experience and lessons from earlier instruction sets. Now, those earlier lessons, The typical complaints are that it's too complicated, too massive, hard to use, and so on.

So its design is extremely simple. As for the entire specification text, if you go and read it, you'll find it really doesn't have that many pages in total. Unlike, say, Intel, or ARM manuals. I don't know if any of you have read them,

but if you go out and work in the future, doing something that deals with this stuff, you'll definitely have to read those kinds of specification manuals. Now, those specification manuals, like Intel's, run to thousands and thousands of pages.

Reading through them once will cost you your life. Of course, you have to go over them again and again. You probably can't read them all at once, but this RISC-V specification manual, it's very small. we'll talk about that later

that is to say, it's only a few hundred pages in total so it's designed to be very simple which makes it well suited for teaching a very good choice for that next, its layering is very clear by clear, I mean it mainly comes down to the concept of privilege levels inside

we'll cover that in more detail later the design is layered and then the third feature is modularity modularity means there is a core instruction set plus some extension instruction sets that's the approach it uses so where exactly does this modularity show up

we'll cover that later as well the fourth main feature is that it's relatively stable This stability, in my understanding, actually still comes from the fact that my understanding of it is still that it came out relatively late

it drew lessons from the experience of its predecessors so the design it came up with that doesn't mean that Intel deliberately wanted to make it this way think about it, right? But why did Intel make privilege level changes so complicated?

It's really still for historical reasons, because a company like Intel, these old-school processor companies, their entire company's development history is the history of CPU development. You see what I mean? It was the first to eat crab — the pioneer,

it waded across the river by feeling the stones. Now, with RISC-V, the reason it can be stable Actually, it's still because he took in and absorbed the experience and lessons of his predecessor. So, in practice, he just once he came out,

the things he changed were relatively few. Unlike Intel, which improves things every year. Why does it need to improve? Because it's still developing, right? Right now, the instruction-level features are actually, relatively speaking,

already relatively stable. And so that, of course, as we said, stability is also one of its advantages. Now, the fourth point, is community-driven development. As for community-driven development, So it's really about right now it's an open standard

that everyone can participate in that kind of thing rather than a single company calling all the shots That's really one of the defining features of RISC-V So, more concretely, let's take a look now First of all, later on in this course,

since we're going to study this instruction set standard, we definitely need to know where to download it from I've already posted the link for you all so you can go and download it Actually, of course, that official document

I've already downloaded it to this which is for this course of ours because this version keeps changing Of course, it hasn't changed in a long time. As for the set of tools we use for programming and development afterwards, what we use is the version listed right here.

Actually, the whole instruction set is split into, you could say, two parts or two volumes. One volume is called the unprivileged instructions. That is what is written here as unprivileged, meaning the unprivileged instructions.

And then there is the second volume, which is called the privileged instructions. Neither of them is very big. For the unprivileged instructions, it's probably only around 200 pages, around 200 pages. If you... you probably don't have that concept yet,

but if you're interested later on, and take a look at the ARM manual, you'll realize that 200 pages is actually a very small number. As for the privileged instructions, It's probably only about 90 pages. So it's a very small booklet.

For students who are just getting started, it might still take some time to read through. But if you already have a little development experience in this area, and you go read these two booklets, you'll find that they're actually

quite easy to get through. Now, these two manuals, we'll be referring back to them often later on, and showing them to you. This... this reflects its simplicity. So next, before we start learning the instruction set, there are some basic concepts

I need to explain to you first. Otherwise, some of the terminology... Actually, that is Otherwise, as we go on later, you might not know what these mean. First of all, we should talk about the concept of the instruction set naming format.

Because we'll be seeing these things a lot later on, things like RV32, RMAR, RV64GC, what exactly are these? So I want to explain to you what this is. So actually, it is a kind of naming format for it. Now, this naming format, we can break it down into a few parts.

Roughly three parts. Let's read it. The first two, RV Actually, it's a kind of prefix. The prefix is an abbreviation. It's the abbreviation of RISC-V. The abbreviation of RISC-V. You see, RV, we know, stands for RISC-V. And then,

after that, the second part, it is followed by a number. 32, 64, 128. This is the concept of the instruction set's word width that we talked about earlier. That concept of word width. We just said this word width, what does it represent?

It's that this processor, its registers, the width of its general-purpose registers, is 32 bits, 64 bits, or 128 bits. Now, the third part, is a string of these letters the string of letters ABCD now, this string of letters actually has meaning

it actually refers to this a collection of its modules because as we just mentioned this RISC-V instruction set has one feature it is modular each module is represented by a letter to denote it for example, what does I stand for what does M stand for

what does A stand for we will cover these later on so, everyone when you look at this kind of stuff in the future When it comes to this codename, you just need to analyze it this way. So this next example is actually simple for us.

Now we already know roughly what it means. Basically, it's like this: the RISC-V, the 32-bit RISC-V, supports the I module, the M module, and the A module. You all get that, right? In other words, say you get a certain chip, and since this chip is customized,

it can be customized, then this chip will definitely tell you in the manual what kind of support this chip provides in terms of capabilities. And from that, you can basically tell that is, it's a 32-bit chip which features does it support

and how many modules is it made up of you get what I mean that's what the codename means so the first few parts are fine but what exactly do these letters and numbers, like IMA, mean this brings us to its so-called modular design

as we just said one design feature of its whole instruction set is that it's modular so what does modular mean well, when it comes to the development of computer instruction sets in general there are actually a few typical design approaches

one is called incremental ISA now, the incremental ISA is our traditional design approach what is incremental ISA It's really a form of version iteration. Let me give you an example. Say I design an instruction set. For instance, the Intel instruction set.

Then my first-generation instruction set might look like this in this diagram. The early Intel x86 instruction set was designed back in 1978. At that time, its instruction set was still relatively simple, because the market demands back then were also quite simple.

It only supported 80 instructions. Then later on, for example, when we got to the 1980s, around 1988 or so, they said our version needed to be upgraded, from version 1.0 to version 2.0. Then 80 instructions might no longer be enough,

so I might need to expand some instructions. Think about it — isn't that the idea? And what it actually expanded Just like when we write code, once this version is upgraded, there is something we call Have any of you heard of this thing called

the concept of compatibility? Because this second version of mine cannot just I can't just say that once I make a second version, I completely throw away the first version. That's impossible. Do you all know why? Because your customers

are not so easy to just follow along and make changes with you. For example, the customers of your first version have already gotten used to those 80 instructions when writing programs. Imagine you suddenly say that a second version comes out

and say those 80 instructions I had before are no longer supported at all and I've come up with a whole new set of instructions then those customers would just lose their minds You all get what I'm saying, right? So what do we do?

I can only say when I'm building a product it has to work like this That is to say, my first version has 80 instructions and then for my second version, I add new ones so maybe there are 140 instructions and when designing those 140 instructions, what's required?

They have to be compatible with the previous 80 instructions Do you still understand what I mean? By compatible, I mean all those programs written by the original customers programs written on the old CPU still have to be able to run on your new CPU

that's the only way the customer will keep playing along with you Do you understand what I mean? This approach is called the incremental design approach. This is also a traditional approach. An approach that's easy to think of.

So this... You see this Intel chip here actually starting from 1978 all the way up to now this instruction set take a look at this picture — it's pretty scary more than 1300 instructions have already been added because with this...

what do you call it this design approach of complex instruction sets you add one instruction add one feature, add one So the instruction set keeps growing. But in fact, you'll notice that even its latest CPU instructions are still compatible with the instructions from 1978.

They are actually still compatible. That's honestly a pretty scary thing. I think you see what I mean. For a company like Intel, its development teams have a really painful time of it. Why? Because essentially, you're carrying that burden and moving forward one step at a time.

Because all these early designs still have to be preserved in the new designs. They have to be kept. So if you go and study x86, looking at some of that code, you'll find that it preserves a lot of old stuff. and you don't know.

It can be really hard to figure out why these things are even there. It's for historical reasons. Because it has to stay compatible, what we call backward compatibility. This is the traditional approach called incremental design.

Now, as you know, this incremental approach brings about a really bad thing, and what is that? The learning cost for people coming after keeps going up, because when you're learning a new instruction set, you still have to take the old instructions

that are still there, you can't just start over. But as we were saying, when RISC-V was being designed, it took this problem into account. Of course, partly because it came along later, so it doesn't have that historical baggage.

You see what I mean, right? It can be designed entirely from scratch. It can be designed to be more modular. It adopts this modular approach. So what do we mean by a modular approach? What it means is, it only defines one basic instruction set,

one module of the instruction set, called the base integer instruction set. As for all the other features, they are added entirely through extensions. And this base integer instruction set, remains unchanged. That is to say, as the spec evolves later on,

its base instruction set stays the same. So if you want to add new features, you do it the way you would with plugins. In software, it's somewhat like this plugin concept. The main program itself basically doesn't change. Now, for the people designing the CPU,

So it's relatively like piling up modules like plugins. So this design, it's possible that my CPU here, no matter how things develop, maybe I won't need those new modules. So my CPU, that is to say, ten years from now, maybe my CPU

still only needs to support 80 instructions. You see what I mean. Whereas your new CPU might just stack things on. So for my new CPU, adding some modules, stacking them on like plugins is all it takes. So that way, it can be relatively

better than a purely incremental approach. That is the idea of modularity. So specifically speaking, the RISC-V instruction set is made up of a base integer instruction set plus multiple optional extension instruction sets. This base integer instruction set,

according to the specification, is mandatory to implement. What does mandatory to implement mean? It means no matter what kind of CPU you design, the instructions required by this instruction set must be implemented. You can't say I only implement the extension parts

without implementing this side. That won't work. So this base instruction set is actually the integer instruction set, the most fundamental stuff for us, for example, what we'll talk about later, like addition, subtraction, that kind of stuff, logical operations.

This kind of thing. These are absolutely required. Once these instructions are defined, they are locked in for good. We will never extend them again. Now, this base instruction set, strictly speaking, is actually divided into four kinds.

We already talked about what these code names mean earlier, and you can probably figure it out yourselves. Two of them are 32-bit, one is 64-bit, and one is 128-bit. Now, as you know, the I here actually stands for what? We call it the base integer instruction set.

Integer — isn't it integer? Right? Its first letter is I, isn't it? Right? So when you see this from now on, 32i actually refers to, the 32-bit base integer instruction set. So suppose I pick up a CPU and say it supports RV32i. That means this CPU only supports what?

The basic integer instruction set. What about E, then? E is actually a subset of 32i. E is actually the abbreviation of the English word 'embed'. You all should know 'embed', right? Embed. Embedded. What does 'embedded' mean? Embedded systems, right?

Embedded systems. E stands for embedded systems. Now, this E is actually built on the 32-bit i integer instruction set, with some optimizations. For example, it has fewer registers. It's well suited for making small wearable devices,

embedded devices. Then there's RV64A and RV128A. I won't go into those. Now, one thing to keep in mind at the same time. That is, there's actually a compatibility aspect here. There's a small compatibility issue on this side.

That is, the 64-bit version is compatible with the 32-bit one. The 128-bit version is also compatible with both 64-bit and 32-bit. So there is a bit of compatibility involved. What that actually means is, if what you're building

is a chip that's RV64I, then something you compiled as 32-bit, a 32I program, can still run on the 64-bit version. I hope you see what I mean here. That's what this compatibility means. These are the basic integer instructions.

Now, as for the optional parts, there are quite a few. By optional modules, I mean there are many of them. I've just listed a few here. That is to say, for example, M, A, F. This M stands for the instructions that support multiplication and division.

It's short for multiplication. Then A stands for atomic instructions. Now these atomic instructions, basically there are some instructions that involve the concept of atomic operations. We should come across some of them later.

We will come across some of them. Then F is float, meaning 32-bit floating-point instructions. D is double. Those of you who have learned C should understand this too. float, double, the idea is actually very similar. That is, this kind of floating-point computation,

we don't provide it as a base instruction. It's provided as an extension instruction. And then there is another one called compressed instructions. Now, for compressed instructions, as we just mentioned, if you don't compress them,

the standard 32 whether it's 32-bit or 64-bit, the length of each instruction as we covered earlier, is 32 bits in RISC-V. But as you know, some embedded systems have fairly small memory, and they want the program to be a bit smaller.

So we can support what's called a compressed instruction set. With the compressed instruction set, each instruction is only half the size of the standard 32-bit one, so one instruction is 16 bits. That makes the program smaller.

Think about it, isn't that the case? So this is what we call compressed that's what it means, the compressed instruction set Then there are many others a lot has been extended in this ongoing process of defining the standard But no matter how it's defined

its base instruction set remains unchanged So relatively speaking, it's a bit more stable Now, for us programmers that's actually quite friendly meaning what you learn today might still be usable ten years from now You understand what I mean, right?

Otherwise, it would be painful So to wrap up, there's a special letter called G This G is not a kind of is not a special instruction set it's actually a general term It actually means as specified in its specification, this G is actually equal to IMFD.

Now, IMFD, as you know, is really just these things: it's the integer instruction set — it supports integer operations, multiplication operations, atomic operations, floating-point operations, and double-precision floating-point operations.

If this CPU supports these operations, then when you write it out, you can make it a bit shorter: just use G to stand in for them. Now that we understand this, when we look again at these special symbols, there probably shouldn't be any obstacle for you anymore.

So if later on you see The specification has some articles that mention things like RV32I. Now, think about it yourself and explain what it means. Is it just a so-called most basic one? The RISC-V experiment is really just what?

It's 32-bit, and it only supports what? The integer instruction set. There are only a few dozen instructions. So, when we cover instructions later, we'll mainly be talking about these integer instructions, because these are the basic instructions.

Once we've mastered the basic instructions, there are some extension parts that you can read through on your own. So, RV32IMAC, read this one yourself. So you can understand this CPU If a CPU says it supports this, then everyone knows

which instruction features it supports. What is this? I wrote it down here. Then there's also RV64GC. You should know that this G needs to be expanded. What does G actually stand for? IMAFDC. Like this C here, C is this. Actually, G stands for this.

So this one, is its naming convention. As a standard being established, this is a basic concept. So everyone must master it. And at the same time, it also reflects this This modular concept in RISC-V. You can see it from here. Now, next,

the next concept we need to understand is the concept of general-purpose registers. In this specification, when we interact with the processor, it's mainly about manipulating these registers, like flipping switches, to control the processor.

Now, for this processor, the specification defines several kinds of registers. One of them is called the general-purpose register, which is also the most fundamental kind. For these registers, there are 32 defined in total. 32 defined in total.

If you look over on the right side here, in this standard specification, each one has a code name. One set is called x0 all the way to x31. So that's 32 registers, right? Now these 32 registers, whether it's a 32-bit CPU, 64-bit, or 128-bit, they're all the same.

If you want to support floating-point operations, then there are separate floating-point registers to use. We won't cover those here for now. Our focus is still on learning these, the most basic general-purpose registers — the 32 general-purpose registers.

Now, 32E — we talked about this earlier. Now, everyone pay attention here. In this diagram, from the specification, from the standard — that is, my reference 1, the reference 1 written earlier, it's from section 2.1 of the unprivileged instruction document.

This diagram was extracted from there. In this diagram, besides the 32 general-purpose registers, it also draws a register called the PC register. As we know, this PC, If you've studied assembly, or studied it on X86, we know that PC is the program counter

— the register that holds the program pointer, right? On some other instruction architectures, we know that this PC register is exposed. What do I mean by exposed? I mean that by writing assembly instructions, you can manipulate this register,

reading it or writing it. Have any of you done that? For example, when debugging a program, you might even modify the PC register, to make your program jump somewhere else. Have you ever done anything like that? Maybe you haven't run into that situation yet.

But sometimes when we debug programs, we use little tricks like that. However, in RISC-V, note that the PC register is not exposed. Even though it's drawn here, Actually, when you're programming, remember that we can't access this PC register.

Of course, this thing definitely exists, because without it, the CPU simply couldn't run. When I demonstrated this to you last class, you saw it, right? So then, why doesn't RISC-V expose the PC register for you to access? It's mainly because of certain

issues in modern CPU design, specifically pipeline design issues. That's more of an advanced topic, so we won't cover it here. But the main thing to know is that this PC register is something we cannot access, but it does exist.

If you really want to get the value of this PC register, it is possible to go and read it, but when you read it, it is read in another special way. Here, there may be some examples later, and I'll introduce them to you. So that's one issue.

As for the rest, as for the rest, well, regarding registers, that is, the general-purpose registers, I'll stop the introduction here. The next concept, once we finish this part, this second lecture will be a bit longer, and we'll cover the material on Risk5

as a whole. So the next one is, these are actually all basic concepts extracted from the spec. If you want to read the spec in the future, hopefully this will help you when you read it later on. Now, in this RISC-V spec, it also mentions a concept

called the hart concept. You'll often see this term mentioned in various places. So what is a hart? Well, this word is actually a term coined within the RISC-V spec itself. In the English language, you won't find this word in a dictionary.

It's not a real English word. It actually stands for what we wrote later on, a Hardware Thread, It takes the first three letters of Hardware and the first letter of Thread and puts them together into this word. Now, translated into Chinese,

it's called the hardware thread concept. And this concept is very important. Why? What do we mean by hardware thread? Let me explain that to you. Now, as you know, on our processors, in the early days, it was generally the processor concept,

the CPU, right? Of course, in some documents, you'll also see the concept of a core, the concept of a CPU core. But they all really refer to the concept of the CPU. Now, when we say CPU, in the early days of the CPU, Inside a CPU,

it can only execute instructions one at a time, sequentially. As for the CPU's execution process, we demonstrated it to you in the last class. You remember, right? There's a CU unit, the control unit. Instruction by instruction, what is this process called?

First, fetch. The first step is called fetch. What's the second step called? It's called decode, right? Then the third step is execute. It does this over and over again. It's like a wheel spinning there. So since it keeps repeating this,

you can actually imagine it as, essentially a stream of instructions, right? executing one after another. So it becomes — there's only one CPU in there. So generally, if there's only one CU, or control unit, there's only one instruction stream being executed in there, right?

Isn't that the case? That's exactly how early CPUs used to work. But as hardware has developed, you get a situation like this: I buy a machine that comes with just one CPU, but inside that CPU, the spec sheet will tell you there are two execution streams in there.

Has anyone heard of this concept? On Intel chips, for instance, there's this thing called hyper-threading. You've heard of it? The concept of Hyper-Threading. Right? So if you open up the Task Manager, say you bought one that's advertised as 8 cores

or a 16-core CPU when it comes back, but if you open the Task Manager and once you open it, if you take a look, the actual hardware processors inside are actually halved. That is to say, Intel's design approach means that within what it claims as one core,

it can run at the same time two execution streams. This is one of its hardware designs. This is a concept on modern processors. Now, once this thing was introduced, it led to a conceptual confusion for us, which is that before, when we talked about one core,

There's only one execution stream inside a core. And then there's no room for confusion. But now, another concept has been introduced. Namely, having two execution streams inside one core. That does cause confusion. Think about it, doesn't it?

So he understands what I mean by this. So when RISC-V was defining its specification, precisely to avoid this confusion, because using the terms core or CPU can easily mix up these concepts, they introduced a new abstract concept

called a hart. That is, they took the concept of an execution stream and fully abstracted it into something called a hart. You can think of it as just a one hart is essentially a virtual CPU. He understands. But in reality, inside an actual processor,

there may be two harts. This is possible. And each hart here, is an independent execution concept. That is, this passage is taken from the specification. Please go read this passage. What concept is he emphasizing? He says, from the perspective of software

running in a given execution environment, that is, from the perspective of software programming, what is a hart? A hart is a resource. This is a very abstract concept, isn't it? He doesn't say it's a CPU or anything like that.

It's just a kind of resource. This thing is an object, and what is it? It can autonomously fetch and execute instructions. That is, this object, what does it mean that it can autonomously do so? It expresses this sense of autonomy,

an independent concept, free from interference. Do you follow what I mean? In other words, it's an abstract resource, a resource that can independently fetch We covered this concept in class. Fetch means fetching instructions,

right? And execute go fetch and execute these instructions. Isn't this what we just talked about, this execution stream? These are two independent execution streams. Do you follow what I mean here? Just picture it as two independent cores.

Of course, it's called a hart, not a core. It's called a hart. Just imagine it as the things we'll talk about later actually a hard... you can imagine it as a CPU running independently It represents an independent, running stream of instructions

Like that maybe that makes it a bit easier to understand So this is the concept of a hart introduced in the specification This concept is very important it comes up everywhere If you confuse it with what we usually call a CPU or processor,

it may cause you some trouble So here I want to emphasize it Now that we have this concept of a hart, we won't use the term processor from now on And what I mean is, in all the specifications, they only use the term hart. without mentioning the concept of a processor,

because it's possible that the processor we get back, might have several execution streams running at the same time. So to avoid this problem, ok, this is the concept of hart, and it's what we want you to grasp. Ok. Now, there's another fairly important concept

called privilege level. This is also something we mentioned just now. We said RISC-V, its specification, it defines a layered concept. This concept, well, the layered concept, is actually relatively easy to understand. That is to say,

what is this thing similar to? Those of you who have studied Linux, we know that in Linux there's often this thing, and it's been taught before, called kernel mode and user mode. Do you understand it? Did a teacher ever cover this in class?

User mode and kernel mode, what exactly is the concept? Did anyone tie it to the hardware and explain this concept to you? That would make it easier to understand what the concept of user mode and kernel mode really is. Now, in RISC-V,

these privilege levels, they're actually related to that user mode and kernel mode. It's not that they're somewhat related. They are very strongly related. So, we say, in hardware, what it means is that when you design a CPU, you have to provide three levels.

You can think of it as a switch. Flip it one way and you're in user mode. Flip it the other way and it's called the three states it defines, called user mode, supervisor mode, and machine mode. So, flip the switch one way and it's user mode.

Put the switch in the middle and it's supervisor mode. Flip it further down and it's machine mode. You can understand it as something like that. So, to give an analogy, for a system like Linux, an operating system like ours, when you run in what we call user mode,

which actually tells us that our CPU is running in the user mode on top of this the switch is set to user mode and when we enter the kernel, which mode do we run in? actually, we run in supervisor mode machine mode is actually a higher level than supervisor mode

in terms of privilege, from top to bottom the level gets stronger and higher so this, uh, machine mode, you can understand it as

uh it's called machine mode, right it's called machine mode. Uh, what is machine mode exactly? uh if any of you have studied, uh... because some of you may have seen that x86 standard x86, haven't you... you've...

I mean, you've come across this concept called real mode and protected mode there's also this concept basically, what it means is that we... This Intel chip, during boot, works like this. Once it powers on, the first thing it enters is real mode.

What is real mode? When it's in real mode, the virtual address doesn't take effect. We've talked about this concept before. When you were learning Linux, regarding that virtual address, when we talked about that kind of concept,

we covered a similar idea. That is, Intel's chips, as soon as they power on, actually enter what's called real mode. In real mode, virtual addresses don't work. The addresses we access at that point, they're all physical addresses.

Remember this concept? Then when the program runs to a certain state, after you modify one of the CPU's registers, the CPU jumps to what's called protected mode. Once you're in protected mode, there's this thing called the MMU that gets turned on.

You've probably heard that term before, right? If you've forgotten, go back and search for it. I'm just using it as an example. Actually, RISC-V is very similar to this concept. That is, when you first power on, our RISC-V processor,

it runs in machine mode, which is the most core mode. This mode is mandatory to implement. It's somewhat similar to x86's real mode, it's just that people use different names for it. And when running in machine mode, virtual addresses are not enabled.

everything runs in physical addresses in the code you access, the addresses written there are all the actual corresponding physical addresses. Then, there is a way to jump to supervisor type. After it's in supervisor type, it's essentially like entering protected mode.

Then virtual addresses are enabled, and at that point the addresses it accesses are all handled as virtual addresses. Now as for user type, it's actually somewhat similar to what we talked about in Linux, which is user mode. So these three modes it defines

actually serve a layered protection role, which is quite similar to the concept found in other chips. So this is the concept of privilege levels in RISC-V. Now, besides these three levels, there is also one called Debug mode. What Debug mode means is

it is used specifically for debugging the CPU. This level has even higher privileges, but we don't use it in ordinary programming. So I'll just mention it here and leave it at that. Now, why do we bring up these three modes?

The point is this: later on, when you look at a chip, it will often say this chip, for example, this chip only supports M-mode. What does that mean? It means that when you program on this chip, you can only access physical addresses.

You cannot enable virtual addresses, so you can't do processing using virtual addresses. He understood what I meant. Then what if it only supports M and U? That is to say, it does not support and likewise, it also does not support the MMU.

But it splits out one level M and U. Then the most complete one supports all three of these levels. So in our for example, a general-purpose chip inside a computer, it definitely has to support all three modes: M, S, and U. Because only after the MMU is turned on

and virtual addresses are supported, can we implement something like the concept of a process. And what I mean is, the concept of a process can only be implemented after virtual addresses are enabled. Otherwise, it simply cannot be done.

Of course, in order to implement this virtual address, you need hardware support like a special MMU, so the demands on the CPU design will be a bit higher. For the operating system example we'll be doing for you later, since we have limited time,

we won't get into virtual addresses. So we, in practice, we'll only work in this mode, that is, we support M-mode and U-mode, two privilege levels. So at least you'll get a feel for this user mode and, it's somewhat similar to kernel mode,

because M is also a kind of kernel mode. Think about it, isn't it? It's also operating in a kernel working mode. So we say Why can it provide this kind of tiered protection? The key, actually, lies in the concept we're going to talk about next,

which is that at different privilege levels, there is its own set of registers. That is what truly achieves the effect of our tiered protection. In other words, it's not just about flipping a switch over, saying we flip it over,

flip it to user mode, or flip it to machine type, and that's it. Not at all. The real reason is that, from the CPU's point of view, at different privilege levels, it has its own independent set of registers, and these different registers,

in different modes, cannot be accessed across each other. For example, if I now take the CPU and flip that switch, switch it over to user mode, then those registers in M mode and the registers in supervisor mode it may no longer be able to access them.

Doesn't that give us a protection effect? Do you follow what I mean? This is what we call kernel mode and user mode. Actually, the most core concept is right here. Now, this set of registers, we call them Control Under Status Registers,

that is, they're called or for short, CSR. Later on, since we'll be writing an operating system, we'll be dealing with CSRs a lot. If you're only writing ordinary user-space programs, In fact, in your code, there won't be any CSR register operations.

But since we need to work with the kernel, when we start writing the kernel, we will come across this stuff. So I hope everyone takes the time to really understand this concept. Now, here, as we just mentioned, at different levels,

under different privilege levels, each one has its own corresponding set of registers. That's what I just said, the U/S/M modes each have their own registers. So here I've briefly listed them out for you. This actually refers to the supervisor mode,

which has its own independent set of registers. And correspondingly, in machine mode, and in user mode, there are also their own registers. But since space is limited here, I won't list them all here. You can go and look them up yourselves.

Reference 2 is the spec we just mentioned. Isn't it split into two parts? Split into two volumes? There's the upper volume and the lower volume. The lower volume is the one about privilege levels. In this spec, there's an introduction to the registers

under each privilege level. Go and take a look. Each level has its own set, so this achieves the protective effect. At the same time, you can see that a higher privilege level can access a lower one, but not the other way around.

That is to say, when your CPU is working in, say, M-mode, and M-mode is the highest privilege level, then when you're working in M-mode, you can access the registers of the lower privilege levels. That's the concept of privilege.

See what I mean? That's pretty easy to understand, right? But the reverse does not work. So for a program running in user mode, if you try to force it inside a user-mode program, by writing an instruction that writes or reads a register from a higher privilege level,

it will cause an exception. That is, an exception will be raised, and your program will crash. This is what provides the protection. Now correspondingly, when it comes to operating the CSRs, those are another set of registers.

So for operating on this set of registers, there are dedicated instructions. These instructions are also provided through extended instruction sets, that is, through the concept of modularization. Now, between different instruction levels,

you can switch. What does this instruction switching mean? If you all actually, once you've learned that, you know how system calls are implemented. What is a system call? A system call is when your user-mode program, the application running in user mode,

needs to access something in kernel mode, right? Doesn't that involve a mode switch? So there are dedicated instructions that help us switch from one mode to another, or here, between different privilege levels. Now, the e-call instruction and the e-break instruction

That's exactly it. Later on, when we get to system calls, we'll walk you through that implementation process. So that's the concept of privilege levels and CSRs. This is also something we want everyone to master — a key point.

Now we have the last two parts left, and once we finish them, we'll take a break. One of those parts is memory protection. Memory management and protection. As we just mentioned, our RISC-V, the RISC-V instruction set specification,

doesn't just define instructions. It also defines a lot of other things. Among them, memory management and protection is also part of its specification. Now, as for this memory protection, in RISC-V it's divided into two kinds.

One is called physical memory protection, and the other is called virtual memory. So basically, physical memory protection is a low-level memory management approach. Virtual memory, you can think of it as a high-level memory management approach.

Because as we mentioned earlier, if you want to enable virtual memory, the CPU must support which privilege level? It must support that S privilege level. Right? So it imposes higher demands on the CPU design. But if I have some simple CPUs

used in a relatively simple scenario, and I don't need to implement virtual addresses, then you don't need to implement that S mode. But, suppose I also want to, in this simple scenario, if I can also provide some protection for memory,

then I can adopt this approach. It's called Physical Memory Protection, or PMP. Physical Memory Protection, is relatively simple. It actually doesn't protect memory using virtualization. It's actually very simple. That is, you can specify,

for example, say this computer has a chunk of memory. Suppose this chunk of memory is this large, say 64K. Then for this 64K, I can divide up this 64K of memory. For instance, this region, I'll call it Xon. Xon means execute-only.

That is, I can put in here, put what? Put instructions. By putting instructions, that is, the content in here I can load it into the CPU and execute it Do you understand what I mean by that? This is called executable Now what about RWX?

It means the content in here can be read, written, and executed Now what about this lock? It means locked which effectively means it can no longer be used So in practice, that means In our programs, we often talk about what? read-only and writable

This kind of access protection can be implemented at this level But under this approach, we are actually still accessing memory through physical addresses but it can already provide a certain level of memory protection Do you all understand what I mean?

This is a simple form of memory management and protection So what about the more advanced approach? Of course, that means enabling virtual memory. The concept of virtual memory, I won't say much about it here, because it's not a focus of our course.

In fact, if any of you have studied Linux, we know that an advanced operating system like Linux, it certainly has to implement virtual memory. And the so-called concept of virtual memory, is that in your program, the addresses you access,

are no longer physical addresses, but what? But virtual addresses. After you've studied Linux, this concept should be fairly clear to you. That is to say, when it runs, between this virtual address and the physical address, Because in the end, the access that gets executed

is still a physical address. So who performs the conversion between these two? It is not implemented in software, but by a special piece of hardware, a dedicated module, namely the MMU. But this MMU is built with special hardware mechanisms,

which cost money to implement. So if, from a cost perspective, you decide you don't want virtual memory, you can simply drop the MMU module, remove it entirely. And in that case, your CPU doesn't need to support Supervisor mode either.

Alright. Now, this thing about memory management and protection This is also a key point in the spec that must be covered. So just get familiar with it. And the last thing in our spec, we absolutely have to talk about, is that whenever you talk about any CPU's instruction set architecture,

you can't get away from this topic. That is, exception and interrupt handling. Right? If any of you have studied anything related to operating systems, exceptions and interrupts are unavoidable. So let's briefly touch on it here,

since I'll go into more detail later, introducing exceptions and interrupts on RISC-V. Now these two sentences are actually quoted from the spec. That is, what counts as an exception, and what counts as an interrupt — there is a difference.

What do we say an exception is? Let's just look at the figure below. What this figure shows is The difference between exceptions and interrupts So what does an exception mean? Let's look at the diagram on the left. So let me give you a general idea of what an exception is.

Basically, over here on the program side, we have a program. A program is made up of instructions one after another. So our program might execute them one by one. For example, when we reach this one, Let me erase this diagram for a moment.

Suppose we reach this instruction, that is, when my arrow points to this instruction. For example, say this instruction does something like, I go and access memory that cannot be accessed. For example, this one. This piece of memory is read-only.

But I insist on writing into it. I write something. Now, everyone tell me, is this legal or not? It's definitely illegal. So, everyone, think about it this way, so this thing, once you execute this instruction, the CPU it definitely knows,

the CPU will, through some mechanism, tell you that this instruction of yours is an exception, that's actually what it means. So, it will So how does the CPU tell you that this instruction caused an exception? It will take your current execution flow,

and stop it, right? After stopping it, it will jump to a special address, to execute a special piece of code, this piece of execution We usually call it an exception handler. This piece of code, normally, is written by yourself. What do you want to do about this exception?

How do you want to handle it? You can also choose not to write one. If you don't write it, then the exception is effectively as if it never happened. You get what I mean, right? So in general, we definitely want to handle the exception.

And after the handling is done, the CPU will go back to the instruction that just caused the exception, and run that instruction again. This is a very key point about exceptions: after the exception handling is finished, it will go back to the instruction where the exception

occurred, and execute that instruction again. And then it continues to run. As for why we do it this way, when we talk about exceptions later on, I'll explain that to you as well. Right now, what we mainly want to do is briefly understand the difference between an exception and an interrupt.

But an interrupt works like this. What does an interrupt mean? An interrupt means that when it is executing this instruction, an interrupt might occur. Once this interrupt occurs, likewise, the CPU will take your current execution flow,

and stop it, switching over to execute a new interrupt handler. Then, after this interrupt handler has finished executing, it will also return, but when it returns, Now notice: does it return like before, going back to that earlier instruction and re-executing it?

No. It returns to the instruction after the one that was just interrupted, and executes that. Now some of you might ask, why do we separate these two cases? Let me briefly explain. What is an exception? What does an exception actually mean?

An exception is essentially, a kind of problem triggered by your own program something going wrong. Something went wrong. And the CPU, in effect, when an exception occurs, gives you a chance to correct yourself. See what I mean?

What do I mean by a chance to correct yourself? It will interrupt your program and make you run a recovery program. This recovery program is our something you write yourself in this form, for example, let me give a more realistic example for everyone.

For example, you all know about the page fault exception, right? What is a page fault exception? Page4 For instance, when swapping pages, you probably already understand this concept. I'm just bringing it up as an example. Let's take the simplest one.

Say I want to trigger a divide-by-zero exception. For example, in this instruction, a division by zero is executed. Once it detects this problem, it will go and run an exception handler, to deal with the problem caused by the division by zero.

and get it resolved. And once it's resolved, you come back and I let you try it again. Still with me? So when you try it again, as everyone knows, once you fix this problem, once it's fixed, isn't this instruction legal now? Think about it.

So after you execute it once more, it clears up the exception. Cleared up. And it just keeps running upward. That's what an exception is. So an exception returns to the current instruction that caused the exception, and lets you execute it again.

An interrupt, on the other hand, doesn't work that way. What an interrupt means is, an interrupt, it's more of a for example, a peripheral notifying you that something has happened then when I execute the instruction I go and handle the request from this peripheral

and once it's handled I go back to the next instruction and keep running so it's as if the interrupt never happened that's what an interrupt means an interrupt, it gives you this effect, that the occurrence of this interrupt is just like it never happened

so it goes back to the next instruction and continues execution so for this program, it actually restores to its original state and keeps running on that's the difference between an exception and an interrupt So, combining the example I just gave you,

and the two points I covered above, think it over again after class. Now, for every instruction set architecture, it will define what happens when an exception occurs— what the CPU does, and what your program is supposed to do.

When an interrupt occurs, what the CPU does, and what your program does. That's what the specification defines. You all see what I mean, right? Now, how exactly is this defined on RISC-V? When we get to interrupts and exceptions later,

I'll give you a more detailed introduction. So, I should say that regarding the instruction set architecture, that is, the RISC-V instruction set architecture, a few of the more important points, I'll just highlight them for you here.

So, that's it for this lecture. That ran a little over time I hope it will help you in your studies going forward Alright Let's take a short break first
