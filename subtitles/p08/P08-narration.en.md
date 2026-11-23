# English Narration Script

Now let's move on to the second part of these assembly instructions, the instruction overview.

Your class is 45 minutes long, right?

Let me check, since we ran a bit over earlier, we'll cover a bit first, and we'll try to let everyone take a break around the 45-minute mark.

So let's take a look at this overview of assembly instructions — that is, an overview of RISC-V assembly instructions. Now, when we talk about assembly instructions, first we need to understand the concept of the objects that assembly instructions operate on.

This is actually a bit similar to what we discussed earlier. Remember in our first class, when we were designing our own set of assembly instructions, we also talked about the concept of an operand. That is what's called OPERAND.

The idea of OPCODE and OPERAND. What we're talking about here is the OPERAND concept. Now, these operands, we can divide them into two main categories. Two main categories. The first category is registers. Because as we know, the basic objects operated on in these instructions

are really the most fundamental ones: registers. Right? So, on this point, we please note, because we in this lecture, we've also noted here, that for this course, this part will mainly only cover the general-purpose registers of RV32i.

That is, there are actually other registers too, but since we have limited time, we can't go into all of them. Later on, we may also use some registers for privileged instructions. When we use them later, we'll cover them then.

Here we only talk about RB32i. Recall this concept, which was covered in the last two lectures. What does RV32i mean? We're professionals now. I can't keep treating you like beginners who know nothing when we discuss these topics.

From now on, when I mention RV32i, I won't explain it. The moment you see RV32i, you should know what it is. It's the 32-bit RISC-V machine's integer instruction set. Do you follow what I mean? Because as we covered before, the design of this RISC-V instruction set

is based on a modular concept. Remember that? Each letter stands for one module of the instruction set. We're only focusing on this part here, because this is the base instruction set, and the integer instruction set is that base instruction set.

That is to say, once you understand these instruction sets, basically, for ordinary programs, you should be able to read and understand them. So let's keep this point in mind. Now, for this integer instruction set, it defines 32 general-purpose registers,

named X0 to X31, with numbering assigned to them. As we go through these things, we should take a look at this, and I'll walk you through the manual. Because later, when you actually go out to work, knowing how to look things up in a manual is one of the most basic skills.

This is a skill, not really special knowledge. No matter which manual you need to consult, you really have to know how to look things up. You need to have this yourself; when what you get in school, may no longer be enough to meet everyone's needs,

then everyone has to master this skill on their own. you can learn on your own Now, as for this manual, as we just mentioned, there's this thing called everyone can follow along and take a look at it together with me under the reference directory,

under this reference directory, there is one called Risk 5 Spec 2019-1213. This is currently the latest unprivileged instruction set for Risk 5. As we mentioned earlier, its instruction set is split into two volumes, one is the unprivileged instruction set,

and the other is the privileged instruction set. The privileged instruction set will come up a lot later when we develop the operating system, we'll be dealing with it extensively, and we'll go through it together then. Let's open this instruction set first

Let's open it with that one Opening this one might look a bit uncomfortable You can all take a look at this This spec is What we'll spend a lot of time studying in this class and later on this We'll spend a lot of time studying it

a spec So this spec, it's Volume 1, unprivileged RSA We've already covered the concept of RSA, right? So the thing I just talked about, the concept of registers, you can look up in many places. So we're As for this chapter, let me just say a few words.

For this book, its first chapter is the introduction. That covers some basics, an introduction to the RISC-V instruction set, the basic concepts. And then below that, it actually follows the modules, that is, one instruction set

module by module, introducing them to you one by one. For example, RV32I, this RV32I, this basic integer instruction set, is right here, covered in this chapter. The focus of this course, actually, is mainly to learn the instructions in this chapter.

You still remember what RV32E means, right? I already covered that earlier. Right? So I won't go into it again here. Now, for this RV32E instruction set, let me take a look. So you see there's a diagram here, did everyone notice it?

This diagram. What this diagram is actually telling us is some of the basic instructions it supports, the basic registers. X0 to X31, a total of 32 registers. These are its general-purpose registers. Now, whatever it says here as X-Lens on the diagram,

pay attention to this. So what is this X-Lens? It actually refers to... It is actually a variable You could say the spec defines XLEN Which essentially means it can be 32 or 64 or even 128 Do you follow what I mean? That is, on different

say, 64-bit machines the width of these registers is 64 bits On 32-bit machines, the width of these registers is 32 bits This differs from case to case Now let me emphasize one more time: from here on, including the exercises later,

everything is based on a 32-bit CPU a RISC-V 32 CPU So whenever you see these registers, you should immediately picture in your mind what they look like a storage space that is 32 bits wide see what I mean That's exactly what X-Lens means

Its bits start counting from 0 which is the lowest bit The rightmost one is called the least significant bit and the leftmost one is the most significant bit So this rightmost bit, that's bit 0 and the second one is bit 1 and as you go further left, it gets higher and higher

Then subtract 1 you all understand what that means, right? That's just 31 Right? Okay, this one, let me briefly go over this diagram How should you read it? x0 is a very special register called zero meaning that this register Now, this register,

this register, so we actually don't you can't perform write operations on it. If you read this register, what you read out will always be 0. Please note that this is a special register. As for the other registers, they are all readable and writable.

And then, if you look at the bottom of this diagram, we also mentioned this earlier, it brings up a register called PC. Now, please note that for this register, in RISC-V, it only tells you that internally I have this kind of register,

but this register You cannot access it. It is actually used internally to store what the PC, right? That is, the position where the program is currently executing which instruction — that concept, meaning Program Counter. But note that this register,

we actually cannot access it directly. So how do we get the current program's PC value? We have a special method to do that later. When we get to it later, I'll bring it up again. This figure, it's really just talking about this thing.

This is what we call the 32 registers, and then, the second point, please note When it comes to registers, that is, our HAT, please note, from now on we will use the terminology we covered earlier, and we don't call it a CPU. Because when we say CPU,

or concepts like Core, that can be ambiguous. We talked about this earlier. I think you understand what I mean. In our RiskFile, there is a concept called HUD. But actually, it is really a kind of, you can think of it as a kind of,

the smallest unit of execution in a processor. So this thing, is actually a bit like the traditional concept of a CPU. But compared to the CPU concept, it's already a bit different. So when it performs this arithmetic and logic operation,

its operand data must come directly from registers. This concept should also be easy to understand, because right at the beginning we talked about this computer this architecture, when we discussed this model, we mentioned that

data moves up level by level, from the hard disk to memory, and then on to registers. Remember this concept? Our arithmetic logic unit, when it does any operations, from the perspective of the CPU, Inside it, what can it access directly?

Only those registers. It cannot, it won't go directly and... for this... that is, take data from memory and directly operate on it. Before it can compute, it can only... if the data is outside, it can only load it from memory first into what?

Into the registers. Only then can its processing unit take the data from the registers and process it. After the data is processed, it is also placed into the registers first. Do you all understand what I mean? And if you want to save it,

it can only be moved out from the registers and put somewhere else. So when we... the instructions for these operations, it's the same story. In the instructions you work with, when you perform operations, if you want to do arithmetic or logic,

addition, subtraction, multiplication, division, you can only fetch data from the registers, and then... the result can only go back into the registers. That's what it means. So that's the stuff related to registers. Now, besides operating on registers,

our instructions also need to be able to operate on memory, because, as I just mentioned, Your data ultimately still has to be fetched in from outside. You have to move the data from memory into the registers first. So for moving data back and forth between registers and memory,

for these read, write, and move operations, there are also some special instructions to handle them. So memory is also something that our instructions operate on as a basic object. Now, when it comes to operating on memory, there are a few concepts you should pay attention to.

That is, for read and write operations, for accesses, what is the basic unit? It's the byte. This concept should be very simple. That is, when we read data, we read it one byte at a time. Each byte actually implies a concept, which is that every byte has an address.

That is, when we've been learning high-level languages, we keep saying a pointer points to that address now, strictly speaking, that address corresponds to the concept of a single byte every byte has an address every byte has an address

right? that's what it means so for our RV32, that is, for our 32-bit machine, the memory space it can access, how many bytes can it access at most? how many bytes can it access? it's 2 to the power of 32 and if you convert that,

that's 4G so this is the basic concept of operands what we just talked about was the assembly, the assembly instructions we operate on in RISC-V some basic objects. Now, next, let's take a look at assembly instructions, the concept of their so-called encoding format.

Now, this concept of encoding format actually, earlier on, when I gave you some introductory overview, talking about the mini computer, when we designed an assembly instruction ourselves, we actually mentioned this concept back then too.

Think back for a moment. Remember when we got to that page, the assembly instruction we designed, we even said at the time, just for show, we actually came up with two instruction formats. Actually, that is related to this concept here.

That is to say, these assembly instructions of ours, because we know that for assembly instructions, It ultimately still corresponds to being translated into those machine instructions. Now, inside that machine instruction, every single

32-bit word, those bytes, those contents, actually carry different meanings. So let's go over the basic concepts once more. Next, we're going to take a closer look at this. So here, for RISC-V, we mainly mean this basic integer instruction set,

it has six encoding formats in total, called the R, I, S, B, U, and J types respectively. What exactly it is, we'll take a look when we get there. So overall, we can see what kinds of concepts are involved here. First, this involves the question of instruction length.

In the specification, there is a technical term called capital-I ILEN1, a name like a variable. You can look it up; it's in the specification. It is actually used to refer to the length of the instructions under different processors.

So for now, that is to say, for us, we mainly target RV32I, which is to say, for 32-bit machines, the length of its instructions is 32 bits. So what does that mean? It means how big one instruction is. We usually tend to use bytes.

So 32 bits is how many bytes? That's 4 bytes. Right? So one instruction is just 4 bytes. So that's actually quite convenient for us to parse. I mean, once you get a machine instruction file, say you're asked to do disassembly,

how do you actually pull out the instructions one by one? You just start from the beginning, 4 bytes at a time, take them out 4 bytes at a time. That's basically the idea. Now this... But some cases may not be like that. For instance, when the compressed extension is supported,

an instruction may be just 16 bytes -- I mean 16 bits. That is, half. So under this situation, our course will not discuss it. Because what we mainly talk about is the uncompressed case. Now the second concept-related point, that is the issue of instruction alignment.

What do we mean by the issue of instruction alignment? Well, we know that this instruction, normally it sits on the hard drive as a file, and when it is executed, we need to take it from the hard drive and do what with it? First, as the first step, read it to where first?

First load it into memory. Right? So the so-called issue of alignment, actually refers to, inside memory, this machine instruction, the address value of the position where it is placed, right? For example, let's say it's 32-bit aligned.

Then its size is four bytes. Its address is always a multiple of four bytes. Its address is definitely 4, 8, 12, something like that. You get what I mean? That's what we call alignment. If things get misaligned, it may cause the program to behave abnormally.

So this is actually specified in the specification too. That is, when you load it, you must load it into memory at an aligned address. The so-called instruction alignment is this concept. That is, it's loaded at an address where...

The so-called alignment here is 32 bits. In practice, that means the address it's loaded to must be a multiple of 4. Of course, we probably don't care much about this process. This is probably handled when the program runs, and for us, it's really just hitting the Enter key.

But in this process, the loading process, is actually done for you by the system. That's the second concept. Now the third one is, when we talk about an instruction, what you get is four bytes, and those four bytes are 32 bits.

So these are bits, and those 32 bits actually carry meaning. So we take these 32 bits, and depending on the encoding format of the instruction, we divide them into different fields. As you can see up here, look, this is a table, right?

For example, this one, for example, we have, this R-type, for the R-type, this is basically like, that's one instruction. So for this instruction we can see it's split into many parts divided into many parts like this this is the concept of fields

each part is called a field each field has a different meaning we'll look at this carefully later when we cover each instruction format we'll talk about what it means what exactly this thing is why do we need to understand this?

Well, it's because we sometimes need to look things up in the manual we need to look things up For example, when you get because in our low-level development we often have to deal with analyzing situations in memory Unlike high-level programmers, who may not care much about this

high-level programmers are probably more focused on business logic issues But in low-level development, we tend to focus more on the hardware, for example, some things in memory that we dig into bit by bit, and sometimes you run into this kind of situation.

So just try to get a feel for how this differs from developing in high-level languages. Now, since we're going to dig into the state in memory, you definitely need to know what each field in memory actually is. As for these specific fields

and exactly what they mean, of course, that's all in the manual, and we'll take a look at it together in a bit. So first, we know it's divided into different fields, and for each instruction, exactly, for example, is it used for addition,

or is it used for subtraction, or is it used for something else? So that's the concept of instruction types. Now, about these instruction types, let's actually mention one thing, which is that this opcode field alone cannot uniquely determine

the type of this instruction. This is probably a bit different from the concept we had back then in that simple mini computer design. There's a slight difference. This opcode field, this opcode thing, cannot uniquely determine whether this is an add instruction

or some kind of subtraction instruction. In fact, it's determined through the opcode and what was mentioned here just now, function3, function this funct These are all terms from the specification. I copied them out. Of course, I need to explain them to you.

funct is actually short for function. function meaning As for 3 and 7, what they actually mean is this field this field occupies how many bits. 7 means it occupies 7 bits. You see what I mean? 3 means it occupies 3 bits. Look at this.

It writes 12 here, what does 14 mean? It means it occupies Bits 12 to 14 12, 13, 14 3 bits That's what it means. So when you read the manual in the future, you should be able to understand it. So all of these, these three fields together,

ultimately determine what exactly this instruction is used for. There is actually a concept here. Let's take a quick look. This diagram is a summary of the instruction types. Let's go through it together. Where exactly in the manual

Let's keep talking, and look at the manual at the same time. So this, in our specification, Where does it talk about some of the instruction encoding formats, these encoding format issues? Well, it actually at the very end, in this chapter at the back,

it serves as a summary chapter, listing them all out for you. You can scroll down to here, there's a section called, this part is called RV32/64G. Do you all know what this G means? You still remember, right? As we go along, let's also quickly review.

G is an abbreviation standing for IMAFD and so on. You still remember, right? It's actually not a specific module. It's actually an abbreviation covering many modules. So this instruction set listing, is actually talking about

its instruction format this chapter we've reached this chapter let's take a look it's chapter 24 you may want to flip through this chapter later this chapter is actually about some formats of the instruction set let me just briefly mention a few things

for example, what is this figure showing what is it about it is about the basic OPCODE mapping relationship actually, it was originally about it is actually about our OPCODE let's go back to this figure our OPCODE this field This FIELD

takes up how many bits? Let's count. 0 to 6, how many bits is that? 7 bits. Right? It actually matches the table we just saw. That table was really about these 7 bits, how they are organized, and what meaning they represent when they take on different values.

That's what it actually means. So in this specification, what does this diagram mean? Let's take a look at it together. It actually looks a bit like our... It is a table, just like our class schedule. the same kind of thing What is it actually talking about?

It's saying that, first of all, we need to know that for this OPC map, bit 0 and bit 1 are always 11. If you look it up in that memory, when you go and check, these two bits are definitely 11. And then, below that, what it's actually talking about is,

apart from these two bits, how many bits are left? There were seven bits in total just now, so taking away these two leaves five bits, right? What should these five bits be? This part is 0 and 1. Then the remaining five bits go from 2 up to what?

Up to 6. Think about it — isn't that right? So as you can see, this is 2 through 6. Now this one is actually talking about bits 2 through 4. That is, three bits of values. And this one is actually about... I mean, its rows are talking about bits 2 through 4.

And its columns... what are they about? Bits 5 and 6. Now later on, we'll... that is to say... For example, let's take an example. Let's scroll down. This table of ours... we'll come back to it in a bit. So just scrolling down...

here we have a summary table. This is actually the encoding format we just went through on the slides, the one that shows the six types in a table. Now, below that, we have the instruction format. For example, let's take a look,

say there's an instruction, like an addition instruction, which is the first one we'll cover later, the add instruction. The add instruction. Now, for this add instruction, its opcode is already written here: 011 0011. So where does this value come from?

Where does this value come from? We actually covered it just now. For this instruction, the lowest two bits are 11. You see that, right? Look at these lowest two bits, aren't they all 11? I won't draw them out. These lowest two bits are just 11.

So what we really care about, is that upper part, these 5 bits. For add, the upper 5 bits, this 0111, where do these 5 bits come from? These 5 bits, we split them into two parts, because just now, based on the table above, what did we see?

It actually is, the table above tells us, the table above tells us, these 5 bits are, 2 to 4 form one group 5 and 6 form another group Do you all see it? So, So, given this situation, then for the value we just had, for this add instruction,

we need to take this thing, and split it into what? Split it into two parts, separating it right here On the left are bits 5 and 6 which is 1, which is 10001 So where does this value come from? Let's look up this table above for this thing

10001 That is to say, Let's first look at the top two bits, 01 01 — do we know who that is? That's it. Right? Then let's look at the remaining three bits. Just turn it sideways. Turn it sideways and look. What is it? 01 what? 01100, isn't it?

I think it's 01100. So the 100 is this one. So this is actually the thing. So all these OP-type instructions, the encoding is actually just 01100. So the add instruction actually belongs to this OP type, the most basic operations.

We'll get to that later, and then we'll come back and look at it again. The point is, we need to learn how to read this table. Of course, the way I'm explaining it, since this is probably your first time seeing it, you might not be fully comfortable with it right away.

That's fine — we have the recordings. Later on, when you actually need to use this, go back and watch the recording, and you'll probably understand what I'm talking about now. This is maybe one of the benefits of having these screen recordings,

at this point. This is just to give you a feel for it, that is, how we look things up in this table. Later on, when we get to the actual commands, we'll still need to refer back to it. So what's the point of looking up these tables?

Well, if you really end up When you're doing this kind of low-level development, I keep stressing that you will go byte by byte, bit by bit, to check the memory. So what these things mean, that's what this document tells you. You need to know how to read it.

There's no way we can cover all of this later on and explain it to you bit by bit, because you just can't memorize all of it. When you need it, just look it up — that's enough. You understand what I mean? This is a kind of skill.

Okay, so that was a quick feel for this so-called concept of FUNCT and OPCODE encoding. And the last one, something else related to the encoding format, Something everyone should understand The RISC-V specification specifies that instructions in memory

are laid out in little-endian order. I don't know if you've covered this concept before. A lot of you probably know what little-endian and big-endian mean. Where did you actually learn about that? You come across it when doing network programming.

Remember Socket? We talked about that. Things like network byte order, host byte order, little-endian, big-endian. Remember those concepts? Actually, the earliest origin of this is right here. That is to say, it's really what we call

the concept of host byte order. Little-endian. Now, for those of you who aren't familiar with it, no problem. We'll go over it for you here. Let's review it once more. Because later, when we look at memory, you need to pay attention to this.

Otherwise, what you see will just be a mess. So what is little-endian? First, we need to understand a concept called host byte order. So what is host byte order? We define it, let's just read it through once, and then we'll explain it.

That is to say, when we discuss host byte order, or let's just say directly, when we discuss the concept of byte order, we're definitely talking about what exactly? it must be about a multi-byte integer rather than a single-byte integer

what do I mean by a single-byte integer I mean the 8-bit kind like a character, that kind of thing you follow, right? a character takes up 8 bits, doesn't it? and that's an integer too in a computer, for this kind of single-byte integer

there's really no concept of endianness because it's just one byte to begin with what order could there be? what ordering issue could there be? right? that is to say, as soon as you're dealing with a multi-byte integer which ones are multi-byte integers?

a short, an int A long, this kind of thing, you still need to understand it. A short just takes up a couple of bytes. Usually a short, takes up two bytes, right? An int is probably generally just four bytes. For a long, it depends on

the specific configuration and so on. So, first of all, I want to emphasize that, that is to say, when you discuss byte order, it must be a multi-byte integer; otherwise, don't worry about byte order at all. Now, when it comes to this

instruction, Why does endianness come into play here? Because as we just mentioned how many bytes does an instruction take up? It takes up 4 bytes so it's already more than one byte, right? This raises the question of how these 4 bytes

are laid out in memory. I think you get what I mean. So this is the problem we need to discuss. Now, as we said, once we understand the basic concept of endianness, we can say there are two types. One is called big-endian, and the other is called little-endian.

Big-endian is called Big Ending, and little-endian is called Little Ending. This is in the documentation. We should know these English terms as well, since we need to understand this stuff. Now, regarding the host byte order we just mentioned

The concept of host byte order may not be mentioned directly in the specification, but in practice it is what we call the endianness issue. Now, this endianness, it is related to the type of CPU. Different CPUs will have different byte order conventions.

So, in our specification, when we talk about the RISC-V specification, it also has to make a corresponding provision for this. So here, what it stipulates is that, by default, it is little-endian. Note this carefully: by default, it is little-endian.

Now, exactly what is big-endian, and what is little-endian? Do you still remember this concept? Can you explain it in your own words? For this question, I still don't have a name list, so I'll just pick someone at random. Let's see,

one of the girls in the second row, I'll just pick any one of you. There are two of you in green, two in blue, right? Never mind, let's go with the one in blue. The student in blue, that shouldn't be ambiguous, right? The one in blue,

that girl there, little-endian, big-endian, do you still remember them? Like, if we say asking you to explain what big-endian and little-endian byte order mean Can any of you explain it? Hmm, roughly right. And little-endian is the reverse of that,

right? It seems everyone has learned this before. So on our side, we don't need to explain it too much. Let's just take a quick look at this diagram. It's really just illustrating what that student just said, that is to say, say we have a number

over here, this is actually a we specifically took an example of one that takes up four bytes in memory, an integer like that As for this integer, we humans usually write it in decimal, for example, a 2874. How large is this number?

ones, tens, hundreds, thousands, ten-thousands, hundred-thousands, millions, ten-millions, hundred-millions. It's two hundred eighty-seven million... Following our human habit, we write it from the high digits down to the low digits.

That's what we say, right? Two hundred eighty-seven million... Isn't this from high digits to low digits? Then the left side, we call it, numerically speaking, the high-order bits of the data. And the right side is called the low-order bits of the data.

Now, the computer, of course, it doesn't like this kind of thing. When the computer represents numbers, it wants to convert them into binary. So below here, it's been converted into a binary So what about binary? It also has the concepts of high bits and low bits.

So in the end, when we're programming, we actually prefer to use hexadecimal to represent things. Right? Now, converting it to hexadecimal, you might wonder why I picked such a strange value. It's because when converted to hexadecimal,

it becomes a very nice-looking number, 11223344. Right? Now, the 11, we consider it the high part, sitting in the high-order portion, the high-order 8 bits. The 44 is the low 8 bits. So once this number is given to you, a question immediately comes up,

namely, when it's in memory, How do we store it? Because we know that to store these 32 bits of data, we can, since storage is byte-based, split it into four chunks to store. Then for each chunk, one byte like this, so it comes down to where

this one byte is placed. Now, a student just explained this, using big-endian as the example, to explain it to everyone. What is big-endian? Big-endian means that the 'big' in big-endian refers to the high-order bits, you see what I mean,

and the low-order bits are little-endian. So what about big endian? It means the high-order data part, that is, what we call this high-order data part. Suppose when we lay things out in memory, we always lay them out from low to high.

Assuming we always lay them out from low to high, that is, when laying them out, assuming we always lay them out from low to high, then so-called big endian means its high-order part goes into the low byte first. So the high-order part, look, 11 goes into where?

It goes into the byte at address A. And the following addresses are A+1 and A+2, right? Because, as we said, memory is laid out in units of bytes. So the high-order byte goes into the low byte first. And as for little endian,

That is, the low bits go into the low byte first. If you keep this rule in mind, it should be very easy to understand what we call the concepts of big-endian and little-endian. Then later on, you'll also be doing it in reverse.

For example, say I give you a chunk of memory, and the data in this memory is let's say, because in your future work, it's often the other way around. That is, you look at the memory first, and what's stored in the memory is because from the low address to the high address of the memory,

the data you see is 1122334, and then I'm going to ask you, what value does this data represent? It's the reverse. In that case, at that point, Before you answer that question, the first thing you should do is ask, for example, say I'm the interviewer,

and I ask you, given this block of memory, tell me what the value inside it is. Can you answer that directly? If you answer directly, you lose. You should ask him first, you should ask back, what kind of CPU is this? What is its byte order?

He understands what I mean by that. So once the question gets to this point, I reckon the examiner won't ask any further. Because he'll figure you understand it. Now, as for our Risk5, it is little-endian, so you need to interpret it in little-endian order.

And when you read it, For example, suppose that this is little-endian what value do you think this data is? Because we know little-endian puts the low byte into the low address first so when laying it out this 11 definitely goes in the low bits of the data

then 22 above it, then higher and higher So if this block of data is in a RISC-V machine environment, then the data is actually like this See what I mean? Basically, you just need to be able to read it both ways That's what's meant by big-endian and little-endian for humans

So why is it called big-endian and little-endian? Actually, the picture here is the big end and little end of an egg It's really just saying that when you eat a boiled egg you know how you crack the egg is your habit big-endian or little-endian?

Your habit is to strike the big end first, or the small end first? What's your habit? My habit is definitely to strike the big end first. Once you strike it, you set it down. Can it stand up? That's the more fun version. Let me tell everyone about it.

It's about big-endian and little-endian byte order. Now, we know that in RISC-V, the instructions in memory, they use little-endian byte order. Just keep this page open. Later, when we do some exercises ourselves, we'll go back and look at this.

So, regarding this, this encoding format, the basic concepts in these things, we'll just cover them on this slide. That's really what all this is about. So now, next let's take a look at some of these specific encoding formats.

As we said, there are six encoding formats. So what is this thing called RISBUG? What does each letter mean? Later, when we get to the specific instructions, we'll go through them again. For now, we just want you to get the general idea,

for example, if someone asks you what R is, well, we know it's actually an English abbreviation. Once you remember the abbreviation, you'll probably find it easier to understand what this instruction format roughly looks like. So R,

it stands for it's actually an abbreviation of the word register so where does its characteristic show up pay attention here in its instructions in its format it has three fields all used to hold registers by these code names like RS

and look, RD now this D here note this carefully this RD here means that this D comes from the English abbreviation of destination you all know what destination means it's the target target in the usual English sense What does that mean?

It means that the final computed result will be placed into this register. Do you all understand what I mean? When we designed that mini CPU earlier, we also when doing addition, didn't we also take that computed result and put it into register 1?

Right? Adding two registers together and putting the result into register 1. You can go back and review that. So this rd, it's used to hold the final result in that register. rs what does the s mean? The s comes from the English word 'source'.

Source, the origin still understand source then it means it for example, if I want to do addition when doing addition A plus B equals C then A and B are the source and C is the destination so this once you see this, you know what this RS ultimately means

and then finally when it becomes that assembly instruction we have a correspondence with this now this register please note each register generally occupies they are all the same each one, if inside it needs to indicate a register

It generally takes one register and uses 5 bits. Now think about it, why 5 bits? It's actually related to what we learned earlier. What do 5 bits mean? It means this: the range it can express for registers is 32. Right? We mentioned earlier,

how many general-purpose registers are there? 32. That's exactly the reason. It won't waste a single extra bit, nor will it be short one. Because at this level, you have to carefully study how to design every single bit. The people who designed the spec

definitely thought through every single bit nothing is wasted So this is the R type meaning that in this instruction three registers are involved its operands involve three registers This is also a characteristic of RISC-V, a feature of this reduced instruction set

meaning that in a single instruction you can involve up to three registers And the more registers involved, as you can imagine the more powerful the operation can relatively be because operating on registers is faster Then the second type is called I

I stands for This I does not mean integer. This I means immediate. Immediate translated into Chinese is called an immediate value. That means in this instruction, it carries a uses this limited number of bits to represent an integer,

to represent an integer. Look at this. For example, its in this I-type format, compared with the R-type, what is the difference? You can take a look. It has opcode, it has rd, it has rs1. There is rs1, but this rs1 is no longer present here.

That is to say, it's missing one register, but, it uses a chunk of content to represent an immediate value. This immediate value takes up 12 bits. How large a range of integers can 12 bits express? You can work that out when you get back.

This is the immediate type. This is the immediate type. Then there's the store type. We'll just go over this quickly later. We'll talk about it when we get there. Store is used to store data, that is, it's an instruction we use later to access memory.

B stands for branch. Branch, as you can guess, is actually related to jumping. Conditional branching. U stands for upper. Next comes jump. We won't go into this one for now. We'll look at it when we get to it. This is what's called the encoding format.

So, as the wrap-up for this little section, we're essentially giving you an overview. And since it's an overview, we should also take a quick look at which ones we'll be learning later on, these assembly instructions. This table of ours,

I've just put together a simple list for you. That is to say, for ours, actually, if you go and look at the spec, the ones we need to learn, the instructions the spec lists out for us, there are actually more than just these. Our space is limited,

so we'll only cover these ones for you. Now, these instructions, we've grouped into several major categories. category by category for you including arithmetic instructions, logical instructions, shift instructions, memory load and store instructions,

and branch and jump instructions. These categories, basically, these five major categories, five major categories. If you have a basic grasp of these five categories of instructions, then when you go and look at some ordinary assembly code,

you should have no problem with it. I think you should have no problem with it. And learning assembly instructions is the same way, that is to say, you know, when you say, "Oh, assembly is too hard to write, it's too difficult for us to learn."

Actually, you don't need to worry. Because in most of your future work, when it comes to assembly instructions, compared to writing, pages and pages of assembly instructions, the chances are actually not that many. Actually, most of the time,

as long as you can understand some short snippets of assembly instructions, that's already enough. You all understand what I mean, right? So, in other words, we don't have to be like high-level languages such as C, that is to say,

for example, writing a few thousand lines a day, which for some students doesn't seem to be a problem. But at the assembly level, writing code like that, that's simply a joke. That is to say, writing thousands of lines of assembly a day,

I said this guy must be joking. What he wrote must have problems. This... Sometimes some of you may have heard some people say, there are programmers like this, he says he only writes a few lines of code a day. That's possible, because he just...

for example, he just wrote a few lines of code in assembly. That is also possible. Okay, so we've roughly divided instructions into these categories. As for the specific instructions, we'll look at them later. So besides what we just talked about,

these are called instructions, which is instruction. The instructions we talked about earlier, so besides instructions, we said in our specification It also defines the corresponding thing called pseudo-instruction. We covered this concept earlier.

The concept of a pseudo-instruction. So if you look at this table, this pseudo-instruction. This column, that is, is the pseudo-instruction. A pseudo-instruction is actually what's called a base instruction here. That is, one pseudo-instruction may correspond to multiple base instructions.

As we just said, otherwise why call it a pseudo-instruction? There is no actual machine instruction that corresponds to it. In reality, it gets translated into real basic machine instructions. That's what a pseudo-instruction is.

Now, the related table of pseudo-instructions. This table here. Is actually not from the specification. The actual specification. You can go take a look at it. In our Chapter 25 of the specification we just mentioned. Ah. Chapter 25.

Chapter 25 is called the RISC-V uh Assembly Programming Handbook, handbook. It's a very small uh, tiny little booklet. In it, it simply lists some uh, commonly used pseudo-instructions. As you can see here, there are commonly used pseudo-instructions

and their corresponding basic instructions, the mapping between them. Later on, when you need to look up a pseudo-instruction, you can look it up here. This is basically an overview of the pseudo-instructions. Later, when we talk about instructions,

we'll also cover some commonly used pseudo-instructions along the way. So that basically covers the overview of instructions. That's all I'll cover for that part. Now, next up, we're going to start the detailed instruction walkthrough.

For the material that follows, you'll probably need to work through it as we go along, doing some exercises along the way. Otherwise, it'll get pretty dry to learn. So make sure you have your computer up and running.
