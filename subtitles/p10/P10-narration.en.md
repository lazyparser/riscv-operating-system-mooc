# English Narration Script

Let's do this exercise together in class, take a look at it.

In our latest repository, all the exercises have been — the exercise document has been moved to the root directory, because some students said they couldn't find it. It was originally in the slides directory, now I've pulled it out, directly in the root directory, you can open this exercise.pdf.

Let's take a look at 5-1. This one is also about the sub command through this exercise we can also see that is, disassemble the execution of this sub command look at the assembly instructions and the encoding of the corresponding machine instructions

and match them against the RISC-V specification yourself that is, just now in fact, matching against the things we just introduced to you be able to match them up with the disassembly yourself that is, that is, for example, we

for example, this command after we run make We use that return edge to debug this instruction After running it That's not right That's wrong Use that make code Which actually calls obj dump So this sub instruction Why is it this one

Try it yourself Combined with the concepts we covered in class See if you can match it up Basically, you need to turn this thing Into its binary form You know, that 010101 And then look it up in the table Look up our This table The sub instruction is right here

See if it matches Including matching up these registers replace it. That's the first requirement. And then, these exercises are all pretty simple. And then, conversely, if we... whenever I give you an instruction, say it's B305, can you, on your own,

using the knowledge we've learned so far, disassemble it back into a assembly instruction? This exercise is very simple. Please try it yourselves. I'll give you a little bit of time. Everyone, focus up. Let's go through it quickly together.

This exercise is relatively simple. The first part of Exercise 5-1, we won't cover the first one. I'll leave that for you to look up on your own. Conversely, that means if it's this, given this, when you look for it, let's take a look at what it says.

It says that the value in memory is B3059500, from left to right, going from low address to high address, in units of bytes. Now, we know that since it's in RISC-V, the machine stores data in little-endian order, so if it's B3059500,

then from low address to high address, what is the actual value? It's actually reversed, right? So first we need to take this value and reverse it. And the other way around, it's 00 95 05 B3 from low bits to high bits. That's actually what this number should be.

Now, for this number, first we need to convert it to binary. For this, we'll just use the calculator to convert it quickly. This one, of course, if you want to convert it by hand, that's also very convenient. So it's actually 00

90505 B3 and once converted to binary, it's really just the string of digits below. Take this string of binary digits. It's actually one value every four bits. Let's split it up. 34 It might be a bit small. Let me make it bigger.

How do I enlarge this? Looks like it's enlarged now. Basically, split it into 8-bit groups. I think I split it wrong here. 1 2 3 4 Wrong. The front is 0. So it's actually eight 0s. So the whole thing is this number. I'll leave this one as is.

And then, after we get to this point, we need to go look up the spec. Now, as for this spec, We just looked at, that is to say, we need to start reading from the low bits, and this CPU works the same way. After it fetches the instruction,

after that fetch, comes decoding. For decoding, it actually looks at the low bits first. So, the advantage of starting with the low bits, is that since it's, it's little-endian code, think about it, the first byte it fetches, is its low bits.

And as for those low bits, 11001, then we have to look at, we know its, all of these RISC-V instructions, The lower 7 bits are all OPCODE. So let's pick out those 7 bits. This is actually 8 bits. It's actually this string of digits:

011 0011. Now, we know the last two bits are both 11. So really we just need to see what 011 00 is. Right? What is 011 00? Let's look up what 011 00 is. 011 00 01 01 01. Even though they're all 011 00 01, which one has 0000? That's the add instruction, right?

It seems to be the add instruction. Let's take a look. It should be the add instruction. Because its first 7 bits are 0. The first 7 bits are 0. 1234567 the first 7 bits are 0 so this part should line up with this then it's 000 this side is 3 bits

1234 12345 so this way it matches up in that case 12345 12345 3 bits 12345 so this matches up up front it's add 01100 01100, right that is to say, if you only look at as we mentioned earlier if you only look at the OPCODE it actually can't uniquely determine

Which instruction is this? Right? It's actually assembled from these. So you need to take this into account too. So all three of these need to be included. Now, from these three numbers once they're determined, it's actually the add instruction.

Alright, once that's settled, it's easy. Easy to handle. So here, in what we're doing for add, this is rd. Over here is rs. rs1. And then rs2. So for this one, we say we convert it into a decimal number, what is 1001? It's 4 plus 1, which is 5, right?

So this one here, 1010 is 4 plus 2, that's 6. And over here, this one is 4, and this is 6, right? No, that's wrong. I read it wrong. I'm getting a bit confused here. 2 to the power of 1. 2 to the... 2 to the... Where did this slide go?

It should be right here. This is 2 to the power of 0, 2 to the power of 1, 2... This is 8 plus 2. 8 plus 2 is... 8 plus 2 is... 10. Is it 11? 01011. So in that case, it should be 5011, right? I didn't calculate it wrong. This is 8 plus 1, so it's 9.

9011, right? Then it should be add and then it's x11 and then x 10 and X9 Is that right? Did I calculate it wrong anywhere? What did you get? Anyway, the rough calculation method is something like that. I mainly wanted to show you how to look things up in this table,

looking it up in reverse. Of course, in many cases, in fact, in most cases, if you have the object code, you can just use a debugger to step back. But, I mean, sometimes there might be times when you don't have machine code it could happen occasionally

so it's still worth understanding but this one involves some basic concepts of encoding formats and we're just going to skip going through it again here so please review it on your own when you get back okay, let's continue with the exercises that follow

and the rest of the course so next up let's look at now that we've covered the add instruction the second one we'll cover is addi the immediate addition instruction and this instruction uses the I-type format the immediate format

and the main difference from the R-type format is that It removes one register. And by expanding the space of one of the registers, it can be used to hold an immediate value. The advantage of this is that it can carry an immediate value

right within a single instruction, so you don't have to go through a register to find that immediate. However, there is a potential issue here. That is, as you know, please note that this immediate value, since it is embedded within a 32-bit instruction,

this immediate value only takes up 12 bits. So the range of this immediate is actually limited. The range it can express is actually limited.

So this is the basic format. We won't go into more detail here. So, this immediate value, you see, it only takes up 12 bits. That is, 12 bits. Now, with 12 bits, the range it can represent is, as written below, it can only express values in the range of 2 to the power of 11.

Actually, the immediate value is also sign-extended before it takes part in the arithmetic operation. This immediate value only takes up 12 bits. So it occupies 12 bits. Now, these 12 bits mean the range it can express is exactly what is written below.

Its representable range is limited to what is written below. It is just as written below. It can only express values from. 2 to the power of 11. From negative 2 to the power of 11 to positive 2 to the power of 11. Now, converting this into an actual decimal value.

That is, from negative 2048 to 2047. A range of values like this. Also, please note one thing. That is, when it takes part in the arithmetic operation. Since this is an addition instruction. Before taking part in the arithmetic operation.

This immediate value will be sign-extended. We covered this concept earlier. What is sign extension? It means that if this number this eleven twelve-bit number if its most significant bit is one, then the bits in front will actually all be

extended into thirty-two bits of all ones, which actually represents a negative number. And if it's zero? Then the bits in front stay as zero. That's sign extension. So either way, the range it can represent is actually limited.

This add i is exactly this kind of instruction. Now, just a note: even though RISC-V provides This addi instruction but it does not provide a subi instruction. Think about why. The reason is actually very simple. It's that, as we know,

subtracting — sub is subtracting — subtracting an immediate value, if you subtract a positive number, it's actually equivalent to adding what? Adding a negative number. Think about it, isn't that right? So RISC-V — we know one of its starting points in designing instructions —

is to be as simple as possible. So if it can save one instruction, it saves one. So it didn't specifically add an instruction for sub. It's a reduced instruction set computer, so it just uses addi in place of subi. That's actually the purpose behind it.

So, now that we've learned these, add, addi, and the sub instructions, that is, addition and subtraction, those are basically it for us. Now, with these instructions in hand, we can actually use them, by combining them and picking certain values,

to express some higher-level uses. So that's how it works. But as for these higher-level functions, RISC-V doesn't provide them with new instructions. Instead, it provides what we call pseudo-instructions. These are exactly the pseudo-instructions we're talking about.

For example, say we want to negate a number, to take its negative value. We define a pseudo-instruction, called NEG. That's the idea of 'negative'. But for neg, it's actually equivalent to really just a sub instruction. But, note the special thing about it:

in the sub instruction's RSE field, it places X0. We know that X0, this register, as we covered earlier, has a special meaning. That is, with X0, writing to it has no effect at all, and when you read from it, the value you get is always 0.

So we know what effect this achieves. It's actually doing one thing, namely subtracting RS from 0. and then store it into ID. We know that if what's in RS is a positive number, then effectively for a number, 0 minus it is actually taking its complement —

getting its negative value. If it's 1, the result is negative 1; if it's negative 1, the result is 1. It's like taking the negative. That's what this negation means. This is the instruction for negation. So what else is there?

Sometimes we need to take two values in registers and, that is to say, take the value in this register and move it into another register. In other instruction sets, there's a corresponding Move instruction. Now, RISC-V, on the other hand

does not provide a dedicated move instruction. Instead, it's implemented using another instruction. In other words, its move instruction is actually equivalent to the addi instruction. As you know, with the i in addi if it turns this last value into 0,

what effect does that achieve? It takes the value of rs1 and adds 0 to it, and then puts the result into rd. Now think about it, isn't the value in rd still just the value of rs1 in the end? Right? So in effect, it achieves the effect of moving

a value between two registers. So, this is another example of a pseudo-instruction. So the NOP instruction The NOP instruction It is a special instruction It actually does nothing Now, in RISC-V, there isn't a dedicated NOP instruction

or a no-op instruction to do this Instead, it just reuses our ADDI instruction So how does the ADDI instruction do it? It uses its rd and rs1 fields, using the special register x0 And then, here it uses the immediate value 0 What's the effect of this?

Think about it You take out the value of x0, add 0 to it, and store the result back into x0 Now some students might say, isn't that still doing something? But as you know, the x0 register is very special. We mentioned earlier that

writing into x0 is it meaningful, or is it meaningless? It's completely meaningless. So when the CPU is interpreting this instruction, it knows that it really can't do anything. So this instruction won't do anything at all. So it might just

go through the instruction cycle of the whole machine, doing nothing but decoding, and the actual action is to do nothing. Now, some students ask about this instruction: why did we invent this kind of NOP instruction in the first place?

Well, it's nothing more than something in the code, It serves as a placeholder. Or in other words, it acts as a no-operation. Sometimes it has that effect. So these are some things based on arithmetic instructions, we can implement some pseudo-instructions,

extending the functionality of our arithmetic instructions. Now, after learning the concepts we've covered so far, especially after studying this addi instruction, some of you may have a question. Because we just emphasized this point,

that is, with the addi instruction, one of its operands is an immediate value. But this immediate value, as we know, since the instruction is embedded within a single instruction, its width is limited, so the range it can express,

is actually quite small. It can only be between 2047 and -2048. Now, if we want to operate on accessing a large number By a large number, I mean one greater than 2047 that is, greater than or equal to 2047 and less than negative 2048

then what do we do? Right? So in this situation, to solve this problem, RISC-V basically addresses how to construct a large number how do we build a large number? Now, to solve this problem, the solution RISC-V came up with is it introduced a new instruction

As we know, a 32-bit number can actually be broken down a 32-bit number can be split into a high 20 bits and a low 12 bits in this kind of arrangement isn't that right? Now what about the lower 12 bits? We can use addi to provide them here.

Because here, it can represent a 12-bit number. So we can first introduce a new instruction, which sets the upper 20 bits of a 32-bit number, and puts them into a register. And then, for those lower 12 bits, we use the addi instruction to add them in.

You get what I mean, right? Just add them in, and that's it. So we fill in the rest, and by filling in, I mean through addi, that is, adding them in through addition, adding them in A plus B style, to fill in the remaining 12 bits.

This way, we can construct a full 32-bit number. Think about it — isn't that how it works? Let's look at an example. So this command, first, let's understand the command that builds these upper 20 bits — the command that sets these upper 20 bits —

what kind of command is it? It's implemented with a dedicated instruction called the LUI instruction. The LUI instruction, actually, is short for load upper immediate. So that's the LUI instruction. Now, the LUI instruction uses what's called

the U-type instruction format. We won't go into the details here. This instruction, the effect it achieves, is that it constructs a 32-bit immediate value whose upper 20 bits It stores it on this side the upper 20 bits, bits 31 to 12, are used for storage

After it's stored in there bit 12 will be cleared to zero and then it takes this final immediate value and puts it into a register that is, it puts it into this RD for example, the register X5 On this side, I've written out an equivalent effect

Suppose I use LUI X5 0X12345 The value in front here is actually a 20-bit number So it's equivalent to taking this number and shifting it left by 12 bits that is, moving it into the upper 20 bits and then leaving the lower 12 bits on the right empty

and filling them all with 0 Why fill them with 0? They're left there for the ADDI to add on top You see what I mean, right? That's what LUI does. The specifics of the LUI instruction and its effect are covered in our exercise, which you can go back and look at.

We won't go over it again in class. So let's look at how it's actually used. Say we want to build a very large number, 0x12345678. Now, for this number, we split it into two parts: it's really just the upper 20 bits and the lower 12 bits.

Right? So we can first take 12345678, or rather 12345, the number 0x12345, and first use the LUI instruction and put it into this register X1 Now, once that's in there, based on the explanation we just gave, we know it actually is

filling the upper 20 bits with this number and at the same time, clearing the lower 12 bits to zero. After they're cleared, how do we fill them back in? We just use addi to fill them in. To fill them in, we take 0x678 and add it onto X1.

Since X1 currently holds this value, adding these two values together, once you add them, as you can probably see, you've assembled one big number, right? Think about it, this is how we use this instruction to load a large number with this effect.

But let's pay attention here, this is actually not as simple as we thought. Not as simple as we thought. For example, let's take another case. Suppose we have a number like this. Take a look, it's 0x12345, and then note that after it there's a

string of all f's. Now, if we want to construct this number, how do we do it? Well, if we follow the approach from just now, some of you would say, that's easy. What do we do first? First we take the number 12345 and load it into the upper 20 bits using the LUI instruction,

right? That's fine. That's absolutely fine. And then below that, we add it in with addi, adding the number 0xff to it. Haven't we put it together then? Think about it — does that work? Let's look back at our addi instruction. How did we define its behavior back then?

Let's quickly take a look. Which sentence here affects our computation result? Pay attention to this, we may have gone through it a bit quickly just now. Have you taken another careful look at the meaning of this sentence? What it says is that before it participates in the operation,

the immediate value gets — what happens to it? Sign-extended. What does that mean? Let's go back to this one from just now. What does this number actually mean? In memory, for the computer, Is it a positive number? See what I mean?

It's actually a negative number. It's actually negative 1. Think about it — isn't that right? Because its sign bit, the highest bit, is After sign extension, it's actually a negative number. And since it's negative, it's not really like what we humans do

adding a positive number to a positive number. It's actually taking the content in XL3 and adding it together with a negative 1. So you're not actually going to get that result. Do you all see what I mean? So doing it this way

can't possibly give you that result. Then a student asks, Well, what if I want to construct that number? How do I do it? So actually, it's very simple. We actually still use these two instructions. It's just that, as we said, we go ahead and...

Think about it mathematically. We want to... add 999. That can actually be turned into what concept? It can be obtained as 1000 - 1. You see what I mean? This 1000, we can actually first... add it to this higher position. For example, we use 1000.

That 1 in 1000 is actually... effectively the carry. We can get it by subtracting 1 from 1000. So, to get this number, we can first construct such a number. We first get 0x12346. This 6 is actually like borrowing a bit at the front,

right? And then we add a -1 to it. This way, we get this result. Think about it, doesn't that make sense? So for us, actually, in this kind of special case, when we construct it, we need to handle this, and we have to use this approach to construct it.

That way, we can get this result. The key reason is that for this number after addadd, we have to do sign extension on it. So this is equivalent to... As you know, this result is effectively the same as sub, the effect of subadd,

isn't it? It's subtracting a 3-bit number. the concept of subtracting one Now, some of you might have a question here isn't this just way too complicated? I mean, when I want to construct a number I actually have to do some special checks

and what do I mean by checks? I mean checking this data in the lower 12 bits to see if it has certain characteristics In some cases it's really simple you just add, but in other cases you still have to use this detour — that is, work around it using subtraction

right? Isn't that way too much hassle? So, in order to reduce everyone's effort in constructing an immediate value, in our assembly instructions it actually adopts a kind of pseudo-instruction called LI With this LI instruction,

LI is actually short for load immediate That's the idea of loading an immediate. It's basically taking an immediate value In effect, it's like directly loading a value into a register. So if you write it using this kind of micro-instruction,

then the real decision is whether to do a direct add or something like a subtract-one operation. This part of the work is really not something you need to worry about. So who actually handles it? In practice, this processing step

is handed over to the assembler. So during the assembly process, the assembler will automatically figure out for you what kind of method should be used for this number to construct the final machine instruction, and achieve the end result.

So later on, when we write code, including in the exercises we did earlier, you also saw that when we copy a value into a register, We don't use that to do the copying ourselves with a dedicated LUI, or an addi instruction. What we actually do is

directly use this pseudo-instruction li, to assign the value. This is where the li instruction we showed earlier in the code comes from. This is a pseudo-instruction. Now, by using this approach, we simply, essentially reduce our workload.

And you can also use this to appreciate once more why we provide pseudo-instructions and that kind of functionality. In assembly, So next, for this li example, let's take a look as well. This example, we were about to cover it, and we might be a bit tight on time,

but let's go through it together anyway. When you get back, take a look at it yourselves. This li instruction, through this example, let's get a feel for what the assembler does for us. Depending on the situation, it turns the li instruction

into different raw machine instructions. For example, right now, for the variable X5, we assign a value called 0X80. Well, we know that The number 0X80 falls within the range of our D-level between 2048 and 2047 Now think about it

in this kind of situation do we still need to bring in an instruction like LUI to construct a large number? It's actually a small number Do you get what I mean? It's a very small number and it falls within this range So since it's within this range

in fact, as we say this LRI instruction if you know how to edit it you can simply translate it into a single ADDRI instruction, and that's it no need for two instructions to construct it You still get what I mean. Because this 8,

the number 80 in Linux 80, can be embedded directly into a single instruction. Through the ADDI instruction, we can attach it straight into x5. Do you still get what I mean? In your code, after you compile it, go back and take a look.

These two instructions, the machine code they generate is exactly identical. That way you can verify what I'm saying. So that's the first case. The second case, suppose the number is something like this. What's the characteristic of this number?

First of all, this number is a large number. It's definitely outside that range. Right? This number is very large. At the same time, what other feature does it have? The twelfth bit of this number, its highest bit, most significant bit,

the highest bit is 0, which means it is actually a positive number. So for this kind of situation, we can use our simplest approach to put it together. What I mean by the simplest approach, is that after splitting this number in two,

you directly combine it using the LUI instruction and the ADDI instruction, and you get the equivalent result. Go back one step, and you'll see that the code generated by this instruction, is just two machine instructions. This is equivalent to that.

After you go back and work on it yourselves, you can try doing this exercise. Let's go back and look at this effect. This is the second case. Now, for the third case, this is what we just demonstrated for you. Suppose this number is also a large number,

and at the same time, after splitting it in two, we find that its highest bit is not zero, but one, indicating it's a negative number. What should we do then? If it's a negative number, then we have to use the approach we just showed,

which is a bit more complicated. We use that method to do it. So through this example, we can see that the same li instruction, in different situations — when you load a different, construct a different number, The machine instructions it generates

may not be the same. That's the effect of this pseudo-instruction. So through this example, on the one hand, we've understood what the Lui instruction does, and on the other hand, we've also understood that from now on, in the end, we

will all use the Li pseudo-instruction to assign values to variables. So what actually happens behind this variable in the end, I hope through this you can get a sense of how our assembly instructions in RISC-V work, which is actually determined by RISC-V's

instruction encoding mechanism. Please keep this in mind. Alright, that's all for the Li instruction. So, regarding this arithmetic instruction, let me check the remaining time. The time seems to be... Do you all want to take a break?

Given the time, it feels a bit... Should we take a break? No break? OK. Then let's just finish it up. Either way, we'll get out early. We'll wrap up this morning. So as we just mentioned, that is to say, the scenario we just talked about,

namely, we want to construct a large number. For example, when copying to a register, say x equals 100, when we need to do an assignment like this, we'll use this li approach. So when do we need to build a large number? When do we often need to access a large number?

As you know, besides this kind of basic addition and subtraction numeric operations, we also often access addresses. And these addresses, as we know, are often also this kind of large number. Especially on our 32-bit machine, when the address space is definitely in a very large range,

so how do we construct an address value? This involves another concept. Let's take a look. When we construct an address, it's actually not much different from constructing an ordinary numeric value. Under normal circumstances,

we can actually use that LUI instruction from just now, that is, the LUI we just talked about. to construct a large number in this way using li to construct a large number in this way to construct an address But as you know, the address constructed this way

is a kind of what? It's an absolute value. Because if you use this method, the address you construct is definitely something directly specified, somewhere between 0 and 2 to the power of 32, that is, the 32-bit range, an absolute value like that.

But sometimes, when we're actually constructing addresses, we also want to construct some relative values. This thing is the relative address, the relative address. Now, for this thing, you'll probably see it later For example, when programming,

you may also run into some related concepts. For instance, have any of you come across dynamic libraries? The concept of dynamic libraries. We know that with dynamic libraries, when they are linked, their addresses all use a kind of relative address,

because, because when they are dynamically loaded, their addresses actually cannot be determined at the linking stage, the static linking stage cannot determine them, so it can only use a relative address to determine them. What this means is that

at compile time, at link time, you need to generate address-independent instruction encodings Instruction. So how is this way implemented? What is it relative to? Well, actually, we say in the compilation and linking technique, we generally use

construction relative to the PC value. Construction relative to the current PC value. So how do we construct an address relative to the current PC value? We often use a... On RISC-V, there is an instruction introduced called AUIPC.

So what is AUIPC? The PC at the end here is actually the PC value. Let's take a look at how this PC value... how, relative to the PC value, it constructs an immediate number. It works very similarly to AUI, it also uses the U-type format,

similar to it. Similar to that, It also constructs a 32-bit immediate And this 32-bit immediate is also built by taking the number specified here and placing it into the upper 20 bits with bit 12 cleared to zero But unlike LUI, there is one distinguishing feature

when it takes this number we said LUI, after constructing this number, it simply saves it directly into x5, right? But AUIPC, it can't do that After it gets this value, it has to take this value and add it to the PC value, and then save the result of that addition

into the destination register. So the effect of its construction is actually equivalent to saying that after it shifts left by 12 bits, it adds that to the PC, and then puts the result into X5. Now, what does it mean to add it to the PC?

Well, as you know, the result you get is actually a value that's relative to the PC, relative to the current PC. As for the original LUI, you could say it's actually not relative— well, it is actually relative too, except that what it's relative to is just 0.

It's really just adding 0. But as we know, once you add the PC, the range it can express becomes much larger, because the range of the PC itself is from 0 to it could be a number between -2G and 2G or it could be a so on this basis, if you further

add another value to it then the range it can express is actually even wider so that is the difference between AUIPC and LUI it is often used for constructing an address especially for constructing a relative address just get familiar with this

because later on we will also use this kind of related stuff so now that we have this knowledge of how to use the AUIPC instruction we can take a look at this example I won't demonstrate it here these instructions are fairly simple

you can combine what we covered in class about its effect and step through it in debug mode Let's see how the values in its registers change. So, similarly, we based on AUIPC, in practice we often don't use it directly, but instead use a pseudo-instruction called LA

to use this approach, to use this command. LA, this pseudo-instruction, it is itself a pseudo-instruction, it actually stands for load address. We often use this command to construct an address, and then assign it to a register,

assign it to a variable. So its effect actually comes from using the AUIPC instruction together with other instructions, combined to generate a This is something we'll use later on, so I'll explain it to you here. When it comes to instructions,

that is, when we need to construct an address, we'll use this thing. As for the usage of this LA command, let's take a quick look at this example. This is the code LA command. Take a look at this command. As for this code, let me briefly walk you through it.

It's very simple. This is essentially the effect of an LA load-address operation. Now, one thing to note when writing it, LA x5 here, this is the target register we ultimately want to load the address into. After that, you simply follow it with a

Usually when we write it, you just directly follow it with a so-called label, or a you can think of it as essentially an address. Now, as for what the specific value of this label is, you don't need to specify it directly when writing the code.

During the compilation process, the linker will automatically determine what this value should be. Based on the final generated executable program's address range, it will decide what this value is. So the effect of this is essentially equivalent to

assigning this address to X5. Now, as you know, JR is something we'll talk about later, and it's essentially a jump instruction. So once you understand this effect, it actually jumps to essentially this thing jumps again to this address,

which in fact forms a loop. That's basically the effect achieved here. Now, this next part, mainly I want you to get a feel for LA, and if you step back and look at it, you'll find it's actually made up of the AUIPC instruction.

That's the LA command. So through what we covered earlier, it's really about whether we're constructing an address, or constructing a specific value, what we're really showing you is how to construct a large number. This is one method provided in RISC-V

for constructing large numbers. Now, regarding these arithmetic instructions, This is also the first category of instructions we talked about. Basically, that's all I'll cover for now. There are actually quite a lot of these instructions.

Including both micro-instructions and basic instructions. So after you get back, you can refer to the table I've given you here, and look at the basic usage of these instructions.
