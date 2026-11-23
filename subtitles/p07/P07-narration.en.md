# English Narration Script

Alright, let's continue our course today. Today we're covering Chapter 5, which is RISC-V assembly language programming. Today's lesson will involve some hands-on operations, so I hope everyone can actually get the environment we set up last time working and start using it.

I don't know if you've gone back and tried the environment yet, but you'll definitely need it for the exercises. Today's lesson is called Assembly Language Programming. It's mainly divided into these five parts. The first part is an introductory overview of RISC-V assembly language.

The second part gives a general overview of the instructions first. I'll give you an idea of roughly what's covered. Then the third part goes a bit further. In practice, that means walking through some of the detailed instructions.

I'll go over those with you. The fourth part covers function calling conventions. And the fifth part covers mixed programming of assembly and the C language. Along with related knowledge. Now, all of you have probably studied some assembly language programming before.

Well, the content is actually quite similar. It's just that Today we're building on RISC-V, this new kind of instruction set architecture. Let me walk you through it. Now, the main references for today's class are listed here for you.

The first one is the unprivileged instruction set, Volume 1 of RISC-V, the Unprivileged ISA. As we go through the lecture, I'll take you along to flip through this instruction set specification together. As for this instruction set document,

the content is in what we sent you, that is, in the online repository, it's placed under reference, under the reference materials section. It's this one right here. When we need it later on, I'll take you through it together. Then the second one

is this online material. So, this one of ours, this... Since we're learning assembly language, we know that with assembly language, when it comes to processing it, we mainly use what's called an assembler, a tool like that, to work on it,

right? And we, we use GNU's assembler, so for this assembler, its online documentation, you can also take a look at it after class. I've listed the link right here for everyone. Now let's get into the main content of our course.

The first part is an introductory overview for you. It mainly covers some basic concepts of assembly language. Let's first take a look at exactly what it is. What is assembly language? Back in our first course, When we talked about the history of language evolution,

I actually already touched on this briefly, right? You all know that assembly language is basically machine language that's been slightly modified, turned into a way of writing that we humans can understand reasonably well, right?

So it's a kind of... assembly language, in English, is called Assembly Language, and it's a low-level language. Now, note this: when we say "low-level", it means that compared to our high-level languages, its level of abstraction is relatively lower.

As for what we mean by abstraction level, you've probably learned some high-level languages, like C, or things like C++ or Java. So where do you think this so-called 'high-level' aspect comes in? It introduces concepts that are closer to

things we humans can understand fairly well, right? For example, some object-oriented concepts, structs, and things like procedures and functions. Now, this assembly language, what we call 'low-level', is actually low-level in the sense that

it is closer to the machine language of the computer's processor. When we program in it, we actually need to understand more, and understand more deeply, some things about the hardware. Only then will it be easier to use well.

It's not like when you've only learned a high-level language, where you actually end up forgetting some things about how the computer really works. So this, when we talk about this assembly language, it has some of these drawbacks, but it also has its advantages.

So, here, let me summarize this for you again. First, let's take a look at the drawbacks of assembly language. Because when we talk about assembly language, since you haven't had much contact with it or used it much, you might feel a little bit afraid of it,

right? And that's actually true, because when you program in assembly language, it's really not that convenient. As we just said, it's a bit on the low-level side. So its drawbacks show up in three areas. First, it's hard to read.

Second, it's hard to write. And third, it's fairly hard to port. Right? Think about it for a moment. That is to say, what do we mean by 'hard to read'? I think all of you should know if you've learned some assembly language, that

when it comes to writing it, it's pretty obscure. Unlike high-level languages, maybe some simple things, that is to say, if you want to implement the same functionality, a high-level language might do it in one or two statements.

But with assembly language, you have to write many statements, because it's basically one statement corresponding to one machine instruction. And on top of that, because it involves things like registers, and various other operations.

So the concept of registers is very tightly coupled with the hardware. If you don't understand the hardware, it's actually quite hard to write. So the second point is that it's hard to write. Right? Hard to read and hard to write.

Now, as for being hard to port, can you figure this out on your own? That is, why do we say it's relatively hard to port? What exactly do we mean by porting? Have any of you run into this issue before? Well, the difficulty of porting is actually quite easy to understand.

As we know, assembly language corresponds one-to-one with machine instructions, basically. So with different instruction set architectures, their machine language is definitely different. Think about it, isn't that so? Different CPU types

Take x86, for example, or, say, ARM. Their registers, the numbers of them are all different, and their functions are all different. So when it comes to operating on these different registers, the instructions and so on are different too.

So writing them is really two completely separate sets of things. Some of the concepts are kind of similar, but writing them is really two completely separate sets of things. Now, given that this is the case, just imagine this.

That is, suppose we want to implement the same functionality. After I've written a set of assembly instructions for an x86 machine, if I take those instructions and run them on, say, an ARM machine, they're definitely not going to run.

In fact, you'd have to rewrite them. So what this shows is that, it's quite hard to port. Unlike high-level source code, with a high-level language, you really only write the code once, and you don't have to care at all about these differences at the CPU level underneath,

right? When you say you write it once, the final translation is actually done by the compiler, which shields you from that whole layer. Think about it — isn't that the case? Now, assembly language, while its drawbacks are pretty obvious,

its advantages are also very clear. Right? Since you're dealing directly with the hardware, its advantage shows up in the fact that it is extremely flexible and powerful. By flexible, I mean you can imagine this: 也就是说, when we usually use a high-level language

to write these programs, in the end, it has to go through the compiler, which translates it into machine instructions for you, before it can run on the CPU. Right? So, since the compiler is the one doing the translation for you,

the optimization process in between, and how the machine instructions are organized, is actually handled by the compiler for you. You have no way to step in and intervene in any of this. But if you want to optimize these machine instructions more deeply,

and bring out the machine's characteristics, its capabilities, and fully exploit them, then you actually still have to go down to the lowest level of machine instructions, at that level, and optimize at that level. So at this point,

you can flexibly organize things on your own, these kinds of operations on the hardware. That is to say, you can bypass the compiler. Put simply, you bypass the compiler to do these things. So this shows a principle: the lower-level a feature is,

the more flexible it is. Right? In fact, if you've used C, and then you use Java, or some other high-level language, like Python, you can feel this too. Right? Because C, compared to other high-level languages, is also a bit on the low-level side.

Because many high-level languages... Actually, underneath it is written in C, to write some of its libraries. So once you have mastered C, you can actually write some more efficient programs, and they'll run faster too. Now, with this flexibility,

what makes it so powerful is that we can operate at the low level and control the hardware directly, doing deeper optimizations. This is where your ability really shows. So that's why these things are after you've mastered assembly language,

there are certain things that in high-level languages simply cannot be done at all, but assembly can do them all. Now some students may ask, so should I actually use assembly? What kinds of things does it do? Since it is so flexible and powerful,

then we say naturally, as we've already mentioned repeatedly just now, that is, it can actually directly manipulate the hardware in a direct way. So the lower level you go, the closer you get to the hardware, in places where you access the hardware,

the more we will use assembly. For example, writing drivers, writing the operating system's bootloader, these kinds of parts will all use this assembly language. But, generally speaking, It won't write large chunks of assembly because if your program

needs a lot of assembly written for it, then actually, we'd actually recommend that you write it in a high-level language instead, because the maintainability will probably be better, and so will the readability. There's one more thing,

which is, if you want to squeeze out the ultimate performance, then, as we just said, since it requires this kind of flexibility, we can actually do that. If your mastery of assembly language is at a really advanced level, then in practice you can

you don't even need to rely on the compiler to do some optimizations you can optimize the instructions completely on your own and that's perfectly fine So this is what we call the application scenario of assembly That is to say, a lot of times, if

you are interested in some low-level stuff if you're really into it, or even sometimes maybe you want to build one yourself, something kind of cool, like a hack, or, to put it less politely, if you want to be a hacker, then mastering assembly

is absolutely essential, because a lot of well, for example, take some little viruses for instance, a lot of them are written in assembly right? Because a virus itself has to be quite small, it can be implanted very easily. So these are some

basic concepts of assembly that everyone should get a handle on first. Now, after saying all this, when it comes to writing assembly, it is also a language, and so, what is this language actually like? Let's take a look at it together next.

So, about this assembly language, one thing to note, is that when you write assembly language, its syntax actually depends, to some extent, on the assembler you are using. Different assemblers the syntax formats it supports are actually a bit different

So on our side, let's define the scope of what we cover. Because when we do the exercises later, the toolchain we provide by default — that is, the compilation tools — actually uses this GNU toolchain, the compiler, the assembler.

So, all of the material I'm introducing here is really limited to — that is, making sure it's syntax that will run on the GNU assembler. Now, some of you, if you take our code as it is now and put it into a different assembler, say one of the Intel ones,

Or if it's ARM's own built-in assembler, it might not recognize it. Please keep that in mind. There is a difference. Now let's take a look at how to actually write an assembly program. Actually, an assembly program is still relatively

not too complicated in terms of syntax. In terms of the overall framework, let's say one, I've written it out here for you. That is, an assembly program, for example, an assembly program, is written as a file, and the suffix is usually a lowercase s,

or an uppercase S. Now, between the lowercase s and the uppercase S, do you all understand the difference yet? I did briefly mention it last time. Remember this. Sometimes you may have noticed that some assembly files have a lowercase s as the file extension,

while others use an uppercase S. In the exercises that follow, at least in this class, we'll basically use the lowercase s throughout. Later on, when we write the operating system, we'll use files with the uppercase S, and you can compare them yourself.

The main difference is really that with the uppercase S, the file can actually contain some preprocessing directives, such as #include, #define, and so on. With the lowercase .s, all it contains is pure assembly instructions. That's how it is.

This is something you can look into. So when we talk about a complete assembly program, that is, a complete assembly program, which is a .s file, what is it made up of? It is essentially made up of statements one by one. Each statement,

we call it a statement. In these slides, I've also written the English terms. This is mainly to make it easier for you later, because a lot of material is still in English and you'll have to read it. So when you see the English,

if there are words whose exact meaning you're not sure of, you can also refer back to these. The things I've listed for you in the slides Because basically, the official documentation is all in English, so I hope everyone will get familiar with these technical terms

—I do want you to understand them. If we only talk in Chinese, it might cause some confusion. Just want to mention that to you. So this is the statements, line by line. Now what format does each line of statement follow? The format is also very simple.

Simple, it's just what I've listed for you here, which is to say it's actually made up of three parts, and the order is from left to right. The first part is something called a label. A label, which is what we'd call a tag or marker.

Now after the label, there must be a colon. So why put it in square brackets? It means that this item is optional and can be omitted. If all three items are in square brackets, that means it supports line breaks, right? That is to say, within this,

we can support line breaks. Now, the second item, called operation. Now, the operation, is relatively important. It is actually the real action. It actually happens here, the operation action. Now, this operation, also has further subdivisions,

which we'll look at in a moment. Now there's another part, which is the comment. A comment is just a note or remark in the code, right? This is something everyone should be familiar with. So the basic syntax looks like this. And the square brackets,

they indicate optional items. Now, in detail, let's take a look at this. We mainly want to focus on the label, especially in the operation part, what kinds of variations there actually are. So first, let's look at the label. As I just mentioned about the label,

that is to say, it is actually a marker or label, meaning that in this file, any identifier that ends with a colon it will be treated as a label As we go through this, let's look at the examples at the same time That might make a stronger impression.

Let's pull up that... If the code is already on your machine, you can open it and browse through it yourself. That is to say, in our... in the code package we handed out to everyone, there is a directory called code, right? Under this code directory,

there are two subdirectories. One is called ASM, ASM is short for Assemble, meaning assembly. So for this part, the examples for this course are all under this one. OS is for the operating system examples. Now, under Assemble, there are many small examples divided up here.

So let's take a look. Right now we'll cover this most basic part, so let's look at this first example, the example in this First directory. All of our files are called test.s, with a lowercase s as the extension. So you can open this file,

and this is our first RISC-V assembly example. In this example, you can see here which parts are what we just talked about, the thing called a label. As we just discussed, this label must end with a colon, right? Isn't that so? So, this...

Like, over here is a label. And of course, on my side here, I've added some syntax highlighting, so it might be a bit clearer to see. Now, this is also a label, and, one thing to notice, this label, it can actually be with other stuff,

it can be written across two lines. Actually, if you write it like this, it's really equivalent to... I mean, these two lines, these two lines are really equivalent to one line. You get what I mean, right? Now, over here, I've written it strictly on one line.

Please note that. It's one complete statement, that is, it fits on one line. So this, what exactly is this label? You can understand it this way: in assembly, this label is essentially just an address. We give an address a name, that's the idea behind it.

Because, as you all know, this code, after it's finally compiled, what does it become? After compilation, it's just machine instructions, right? So this kind of, this line here, In the end, it will actually be compiled into a single machine instruction.

Now, as for a machine instruction, as we know, the process of running a program on a machine, we already covered that in the first lecture. Let's review it. Basically, every machine instruction is loaded into memory, and as it sits in memory,

doesn't it have an address? Remember that animated demo of ours? A mini, that mini CPU, how does that mini CPU run? Instructions are loaded in one by one and executed, right? Every instruction has an address. Now, this stop label,

you can actually think of it as It adds an address to this instruction, it attaches an address. So attaching it here, presumably means we'll later need to use this address to reference it. Some instructions don't have an address attached in front,

which probably means we don't care about the address of that instruction. Now, suppose sometimes we do need to reference the address of this instruction, then we can put a label right here. Even for some variables we define, we can also add a label.

Because everything in memory, like a global variable, may also have its own address. Think about it, isn't that so? So a label... in assembly, you can really just think of it as an address, and that's all there is to it. Alright.

That's the concept of a label. Very simple. Now moving further down, we have the operation. The form of an operation is a bit more complex, with quite a few variations. As for the operation, if we summarize it, it can take four forms.

The first one is the instruction. Because, as we said, the most basic thing is, in our assembly language, one statement actually corresponds to one machine instruction. So an instruction is called an instruction. a string that directly corresponds to binary machine instructions

So in the example we just looked at, which ones are machine instructions? Take a look, everyone — which ones are machine instructions? Strictly speaking, all of these are actually machine instructions. But correspondingly, when we talk about

the concept of an instruction, in these statements, this is the instruction. We'll learn later that the identifier add, this string, is a code name for a basic instruction in RISC-V, a basic instruction in RISC-V. This is what we call an instruction.

Now some of you might ask, what about things like LRI here? Aren't those instructions too? They are instructions as well, but We'll talk about that later. This kind of instruction, we call it a pseudo-instruction. It still differs

from the native instruction in some ways. Now, as for pseudo-instructions, on our course materials, it's written as, the English term is 'Pseudo Instruction' —meaning pseudo, a fake instruction. So what... why do we call it a fake instruction?

The thing is, when you actually go look up the RISC-V manual, it actually... it doesn't correspond to a native This RISC-V instruction, rather, we say it exists purely from the perspective of writing code efficiently, purely for the convenience of

our programmers when writing code. That is, it takes some hard-to-use instructions, basic instructions, or some variations of instructions, or some combinations of native instructions, and wraps them into a simple one, giving it an alias.

You get what I mean here, right? It's given an alias, and then, to make it easier for us to use. So this pseudo-instruction, it's possible that a pseudo-instruction is Perhaps it's that is to say, as an assembler after we've written

this assembly code, don't we hand it over to that Example assembler to process it? What does the assembler do? It takes this assembly language and translates it into what? Converts it into what? Doesn't it convert it into machine instructions?

Think about it, isn't that right? Now, this assembly instruction, when it sees this kind of pseudo-instruction, it will break it down further. It is broken down into what we covered earlier— real instructions that are then processed.

This is called a pseudo-instruction, and it is part of the assembly syntax as well. In our instruction set, you'll see that in the specification there is a definition for it too, but it is actually an alias for an instruction.

You can think of it this way: a combination of instructions, or an alias for an instruction. As for what pseudo-instructions really are, we'll go over that gradually later on, and you can take your time to understand it then.

So as for which pseudo-instructions exist, we'll naturally know once we've studied them. So here I'll just tell you directly: for example, this is a pseudo-instruction. It looks very similar to that instruction, but actually, after it is processed by the assembler,

It actually gets translated into some other real instructions. These are called pseudo-instructions. Now, what else is there? There are also things called directives. You can look this up when you check the online documentation,

it's all in there. So after I've talked about it, it will probably be more convenient for you to study it on your own. As for directives, they are called... In Chinese, we translate it as "指示" (zhǐshì), meaning instructions or directives.

That's because the word 'directive' in its original meaning means instruction or directive. But if you feel this Chinese translation... because our Chinese translation... Actually, it's not official either. It's just my own referencing some things online,

or my own understanding, that I wrote this with. So if you all if you feel this translation doesn't quite suit your taste, then just remember the original English text. Because memorizing the original is actually better. It might be more convenient.

If you have a better understanding of the original text, you really don't have to force it into Chinese. Now, this fence instruction, we call it a fence operation, and some books also call it a fence operation. So this Chinese translation has quite a few variations.

Please pay attention and distinguish between them. But in the original text, it's called a directive. What is a directive? What are its characteristics? Its characteristic is it must always start with a dot, something that begins with a small dot.

Let's take a look at that example from just now, which ones are called directives. Everyone, take a look at which ones are directives. This one is a directive, right? This kind is also a directive. Do you all see it? These are all called directives.

Over here, I've marked them out as well. This kind of directive, it actually does not belong to RISC-V instructions. It's actually part of the specification itself. It actually belongs to syntax that the assembler defines itself.

Syntax it defines on its own. It's really meant for the assembler to read. Based on your directives like this, the assembler will actually do some special processing. It will perform some special processing. It's a bit like preprocessing,

things like include, or define, that kind of stuff. But I'd say they're not exactly the same. But it's something used by the assembler. What exactly do these things represent? We'll take a look at them when we come across them later.

For now, we're just going over some general concepts here. Like this .and thing, you can probably guess what this kind of thing is for, right? What does .and mean? That is, telling the assembler that our program here is done. Still with me?

If you put this .end after it and then write some more stuff, you can try it out later— it really won't get assembled; it simply won't be processed at all. Now, this macro thing here— can you tell what this is? What is it defining?

It's defining a macro. Right? A macro. This .endm here simply marks that everything between this macro and this .endm is actually defining the macro. So what is the macro? We'll see it again in a moment. The .text part, is really just telling everyone that

the instructions we generate, need to be placed into that thing called the .text section. You still remember this concept, right? Last time, why did we go on and on about those compilation and linking concepts, the ELF stuff? Because we actually use all of that right here.

So when you see these things, think back to what we covered earlier. Knowledge is always built up — the earlier stuff lays the groundwork for what comes later. As for .global, it's a bit like the thing in C, like when you define a global variable.

It's telling you that this thing here, this identifier, Now, this 'start' thing, it's a global thing. So what exactly is this 'start'? It's yet another address. You can think of this address as just the name of a function. You see what I mean, right?

You can understand it that way. Because this function 'start' here, it's visible externally, so they just use it like this, somewhat like what you'd define in C, the concept of a global variable. extern It's a bit like that extern,

a bit like that extern, but of course you can't compare them completely. Can't compare them, yeah. This is the third thing we've talked about, right? pseudo-op This is called a pseudo-instruction Please pay attention to this kind of pseudo-instruction

It does not correspond to an actual instruction It only exists in the assembler If you go look at the RISC-V spec or in Intel's instruction manual you won't find these things there You can only find it in the GNU Assembler's manual

You'll see its definition in the assembler manual So please note this And then, there's more That is, in the Operation field what else can appear? You can also have what is called a macro This is a macro What is a macro? It's actually quite similar to the concept of macros defined in C

As we know, in C we can define a macro and then in the C code use the macro to substitute for it Right? This concept is pretty simple, isn't it? Look here. We defined a macro. do nothing Actually, I didn't do anything here. The name of this macro already tells us

that this section below these two instructions really do nothing. Because this nop, we'll learn about it later, is actually an empty instruction. It doesn't actually do anything. We'll talk about it when we get there. Anyway, the point is, later on,

look here, we invoked the macro do nothing here. Now, when the assembler processes this piece of code, it will take the macro do nothing and expand it. This is the same concept as preprocessing in C. After expansion, as you can see, this becomes two nop instructions,

and nop itself is again a microinstruction. So microinstructions are then translated into machine instructions. We'll take another look at that later on. So it has this kind of macro substitution, this kind of effect. This is what happens when a macro is called,

that is, this again. All right, that's... So basically, with regard to the .option directive, the things involved here, we've covered for you. Of course, there are still some more complicated parts, which we won't go into. Basically, we've just

touched on the basic ones we'll be using, here for everyone. Now, the last part What we mean is comments. As for comments, there are actually many ways to write them. Including the kind that looks like C comments, those are supported too.

But in assembly language, the most commonly used form is one that starts with a sharp sign. Please keep that in mind. I mentioned this before, it's just the common convention. There are others. If you see other forms later on, like using a semicolon,

or double slashes, or something like that, don't say the professor never taught it. This is just because on our side, for simplicity, So we've listed for you the most commonly used way, which is to start with the hash symbol. When you start with the hash symbol,

in C it doesn't mean a comment, it's a preprocessor directive. But in assembly, this is a comment. See, here it starts with the hash symbol, and over here they all start with the hash symbol. That is to say, when it's being processed,

starting from this hash symbol, this line is processed line by line, and everything from this hash symbol onward is considered a comment. So it's the same here. For this line, the beginning is still some normal operations, but from here on out,

the color all turns into this purple color. Right? It's all covered now. So basically, that means once you understand this, you can basically write your own very simple assembly file quite easily, right? Take a look. So, about assembly,

this basic, super-basic what do you call it, syntax stuff, we'll stop our introduction here.
