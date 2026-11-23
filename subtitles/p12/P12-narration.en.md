# English Narration Script

So today, first, let's take another look at conditional branch instructions. Conditional branch instructions are called Conditional Branch Instructions in English. They are mainly used in our code — as we know, besides sequential execution, we have things like the most basic conditional judgment in C, the if-else statement, which is actually carried out by these assembly instructions, these machine instructions.

So in the conditional branch instructions of Risk5, there are several kinds, and let's take a look. There are BEQ, BNE, BLT, and these names actually have meanings. As for their specific meanings, we can see them here, which I have also listed for you.

So for BEQ, it is actually branch if equal. What that means is, what does it mean? It checks whether two numbers are equal. If they are equal, then it jumps. Branch means to branch, forking means jumping. The others are similar, BNE

Now, once you remember this English term, when you memorize this, the name of the command becomes much easier to remember. This is one thing everyone should pay attention to. And there's another thing, which is these conditional branch instructions,

the encoding format they use, we call it B-type. So why is it called B-type? Now you all understand what this B means, right? It actually stands for branch, it comes from conditional branch instructions. This encoding format, this encoding scheme,

you can take a look at it. There's a diagram down below, which I've laid out for you. As we can see from it, bits 0 to 6 are still, that is, the lower bits 0 to 6, it still holds the opcode right and then there's also two fields that hold registers

RS1 and RS2 now as for function3 it's used together with the opcode to uniquely identify the type of this instruction and then there's also an immediate value as you can see this immediate value now some of you might have some confusion about the

encoding method of this immediate and feel a bit puzzled which is that this immediate value is not kept together in one piece do you all see that that is, for the immediate value as a whole It is a The numbers written here are the lowest is bit 1

and the highest goes up to bit 12 So you can see that for the full 12 bits of data they are not placed together They are actually distributed across the entire encoding format, in two separate places And in the two places, the format is actually still

bits 1 to 4 are here then it jumps over here and puts bits 5 to 10 bits 5 to 10, then it jumps here again to put bit 11, and then bit 12 is placed here They are not placed consecutively Do you see that? Don't be surprised by this

As you all know, this is a machine instruction. The way machine instructions are encoded, it's really mainly considered for the sake of the underlying hardware's processing efficiency. So this encoding scheme is designed entirely for

the processing efficiency of the underlying hardware. Now, when we're programming, we don't actually need to worry about this. All we need to know is that these two parts together make up an immediate number -- a 12-bit immediate value --

and that's enough. That's one thing. And then there's another, which is this: we know that this So besides knowing that these conditional branch instructions, they operate on certain — that is, whether things are equal, not equal,

they're divided into several groups like this. Right? As you can see, one checks for equality, and one branches if not equal. Then there's another one, which branches if less than. And another one that branches if greater than or equal.

Less than and greater than or equal — we know those are a pair, right? So that's one. And then there's another one, that is to say, there's also this one here — you see, apart from this BLT, There's also a BLTU. And for BGE, there's a BGEU.

Now the only difference is that it has an extra U at the end. So what does this U mean? It stands for the unsigned concept. That is to say, when they If there's a U added, it means that when they compare, the comparison is done as unsigned numbers.

In other words, the sign bit actually, you can just treat everything as a positive number. Now, if there's no U, it uses the signed representation. So in this case, this immediate value's— I mean, this isn't an immediate value,

it's the number stored in the register— if it's a positive or negative number, it does take part in the comparison. That is, the comparison is performed as signed values. This is when these instructions were designed. We need to pay attention to this.

So in total, the classification goes like this. It ends up being six in total. Six conditional branch instructions. You can actually memorize them in pairs. Now that we understand this, the last point is that because in the end it needs a jump,

when this jump happens, this target, the target address of the jump, how is it calculated? This is something we need to understand. As you can see, the way it is calculated is right here. We've listed it out for you here, that is, the method for calculating the jump target address.

And how is it calculated? It is based on the immediate value in the encoding. Multiply this immediate by two. After sign extension, pay attention here, read carefully. Of course, what I've done here is take the English from the specification

and translate it into Chinese for everyone here. Once you've understood this, go back and take a look at the standard itself. That might give you an even better understanding. So, first multiply this immediate by two, then after sign extension,

add it to the PC value, and only then can you get the final target address. So what does this multiplying by 2 mean? Actually, what it means is it actually shows that the immediate it finally gets is definitely a multiple of 2.

Do you all understand? It's definitely a multiple of 2. like 2, 4, 8, values like that. Now why is that the case? It's mainly because it's that as we mentioned earlier, in instruction encoding, when instructions are laid out in memory,

there's this concept of alignment. That is to say, each instruction may follow this... I mean, that is to say, of course, here in our case, since the exercises we'll do later don't involve these compressed instructions, but we know

that, as we discussed earlier, this RISC-V... It supports this compressed instruction format. Now, as for compressed instructions, each instruction compared to our normal ones today, for normal instructions, we know how many bytes is one instruction?

We covered this earlier, you still remember, right? It's 4 bytes per instruction, right? But with the compressed format, it's an instruction extension, specifically the C extension. With the C extension, one instruction is half of that,

and since it's half, it means a single instruction actually takes up 2 bytes, and multiplying that by 2, That's actually what this case is about. At least that's how I understand it. You can all look at it this way. Understand it like this.

So, since this instruction has a minimum alignment of two bytes, then when we encode it, that last bit behind it, that is, the one corresponding to two to the power of one, that bit, can actually be omitted. We don't need to write it in.

Think about it, right? Because it's always zero. Think about it — the lowest bit is always zero. In other words, its address, the last bit of the address is always zero. So this bit doesn't need to be part of the encoding. So this way, we can save one

save one bit. You see what I mean, right? So when they designed the instructions, this is how they saved space. So if you look at it, if you look at this, this immediate, this number here, you can see it starts from 1, encoding begins at bit 1, right?

So right here, there is no bit 0. Notice that, everyone. No bit 0. So that means these 12 bits are actually the upper 12 bits. In fact, this whole address is really a 13-bit value. Now, with 13 bits, we also have to consider sign extension.

That is, it can represent both positive and negative numbers. So its actual range is from negative 4096 to 4094. You can work this number out for yourselves when you get back. Based on the related concepts I just introduced to you,

you can guess why it's this range. Why is it this range? Anyway, it eventually produces a value, and note that this value is added to the PC. So when it jumps, the jump is relative to the current PC value. Please keep that in mind too.

In other words, based on the current PC value, it jumps to an address roughly 4K above or below, jumping to a new address to fetch the value. So that's how the target address is calculated. It's also an important concept for understanding jump and conditional branch instructions.

Then, when it comes to the actual programming, when you're writing code, Actually, to make things easier for us programmers, we don't actually go and do it like this afterwards, writing an actual number. Because if you were to write these values directly,

it would mean that when you're programming, you'd have to work out the absolute address of every single instruction yourself. Think about it, right? There's really no need, because we have the compiler to do that calculation for us.

So when we actually write the code, we don't write the immediate values directly. We use labels instead. You all remember the concept of a label, right? That is, a label. We talked about this earlier. What exactly is a label? It's really just the concept of an address.

It's just that, you don't have to write this address yourself. It's at compile time With the compiler and linker working together to help you help you compute this value and generate this machine instruction. Does everyone understand?

So when we actually program, we just need to write that label. We can look at an example in a moment, and you'll understand this concept. So in summary, for these conditional branch instructions, that's all the related concepts.

What I just covered are the six basic conditional branch instructions. Now, based on these six basic conditional branch instructions, our assembly manual's specification actually also defines some commonly used pseudo-instructions for us.

We also learned the concept of pseudo-instructions in the last class. In fact, a pseudo-instruction is essentially just an alias for a basic instruction, right? For our convenience, you can see that for some special some special operations

For example, things like less than or equal to, these are actually just equivalent instructions derived through transformation. You can go back and look at them yourselves. We won't go through them one by one here. So those are four instructions.

Now, what about these six instructions below? Actually, these six instructions are quite special. They all perform a special comparison with a particular number, they all do a special comparison with 0. Because, as we know, when we program,

a huge number of operations compare against 0. I don't know if you've for example, when programming in C, ever noticed this pattern. In your code, there are lots of places where you compare against 0. So we need to take this kind of

Instructions that compare against 0 we define them specially Of course, they are defined as pseudo-instructions just to make them convenient for us to use For example, we often have loops, for instance if you still remember that loop of yours, that negative loop

in the end, right say you keep incrementing or decrementing until it goes down to 0, and then it compares and also, sometimes say a function's return value is also compared against 0 right? When we do a system call generally, returning what number means success?

Remember that? In our C language When you make that system call, it generally returns 0 to indicate success. Right? When those of you first started learning C, you might not quite have understood why on earth we use 0 to mean success.

It's because 0 is a very special value. Any non-zero value means what? Failure. A failure code. Right? So very often in our code, in lots of places, we compare against 0. So, it's the same in assembly. The comparison against 0 certain special operations

are also grouped together separately and a set of instructions is placed here of course these are pseudo-instructions so to sum it all up in our code every place that involves a condition check will use this kind of jump branch instruction

conditional branch instruction now the basic syntax is not complicated so we'll stop here for this part now let's look at an exercise for example down here below is a piece of code written in C you can think about it that is to say

what kind of assembly instructions we can use write out an equivalent form Now, of course, the source code is something I've already provided to you in the exercises not in the exercises, in that ASM code directory But for now, we can, in class,

think it through ourselves. What do you think, using something like this, first, the first instruction assigns a value to i. So since we're writing assembly instructions, as you know, these kinds of variables in C, can actually be seen here as just a what?

Can't they be seen as a register? Think about it, can't they? So we can actually assign a register, take the initial value of this register, and set it to 0. Now, combining this with the knowledge we learned earlier, as you know, for a register,

If we want to assign it a value, which instruction can we use to do that? We covered this before. There is a dedicated pseudo-instruction called li. Remember? What does li mean? It stands for load immediate. It loads an immediate value.

Right? What's the advantage of using this instruction? Remember? When we construct a large number, it's very complicated. Remember what we covered in the last lecture? When we write code, we don't need to construct it specially ourselves.

We use a pseudo-instruction. li is a pseudo-instruction. It's handed off to the assembler. And the assembler... it will help us, based on this actual number, generate one or more actual machine instructions, right? to construct a number,

what happens if it's a large number? For a large number, there are several cases again. Remember that? Recall everything you've learned before. Now, since we're here, the number is still very simple. It's 0. So we just use li to define a register

and set it to 0, and that's it. Now below, this y loop, at this point, this involves condition checks. Did everyone see that? Right? I mean what is this value being compared against? What value is this i being compared to? Under what condition

will it do something? He understands this concept. So here, it's essentially just a conditional comparison. We keep incrementing this i over and over. What method can we use to implement the increment? We learned earlier that basic kind of

arithmetic operation is just adding one. Think about it, isn't that right? It's just add — the concept of add, right? an addi, or just an add and that's all it takes to implement it. Then, we keep adding to this i, and after adding it a certain number of times,

we take it, and each time we add once, what number do we compare it with? We compare it with that, the value 5. Right? So where is this 5? This 5, we said 5 is a constant. When we compare, we still have to put this 5 into a register to do the comparison.

Think about it, isn't that right? So besides needing to designate a register, to hold this besides the variable i, we also need another register to hold the constant value 5 because inside the CPU, it's impossible to compare directly

from memory, it has to take the values and put them into the registers first, don't you think? So for this code, don't you need at least two registers? Right? So, then what? Then you take these two registers, and keep comparing and comparing their values.

When the two values when they're equal that tells us whether to keep looping we don't loop anymore think about it, right? so, this when we write it it's really just about this much code feel free to take a look that is to say combined with our earlier analysis

building on the analysis we just did first of all we define two variables one is 5 X5 we use this X5 thing this concept of X5 as we covered earlier in RISC-V, the registers all have numbers So how many registers are there in total?

There are 32 registers in total. Right? 32 general-purpose registers. Use whichever one you want. Of course, you can't just use them however you like. In fact, they're not completely free to use. We'll talk about that later. But at this stage,

we can treat them as free to use. Let's use X5. For example, suppose we use X5. Now, with X5, well, this X5, in the C code, actually corresponds to i. Because later, what are we doing with this X5? We keep adding 1 to it. You can see that, right?

So this 5... We need to put it into a register, because when it comes to the final comparison, the CPU is actually taking two registers and comparing them. So this 5 also has to go into a register. So we'll define another X6, and put this 5 in there.

So this X6 essentially represents this 5, right? And then, down below we need to repeatedly increment this i, which means adding 1 to x5. And each time we add one, as I just said, I do a comparison, comparing x5 with x6. We just learned this bne earlier,

what does it mean? It's branch. What does ne mean? It means not equal, right? not equal So if they're not equal, we just jump jump to loop. And loop, as we know, is really just an address. So from here, it actually starts the increment by 1 for the next round.

Think about it, right? Because this x5, starts out as 0, after one increment it's 1, and they're still equal. If they're not equal, we jump. Do you follow what I mean? Then it increments by 1 again, and eventually it will reach a point,

when does it become 5? Think about it, right? And when it becomes 5, it won't jump and just continues straight down and jumps out of that Y loop so that's essentially the effect now, what else is there let's try it out so here's the thing

we can see that on our side, we don't write an immediate address we just write a label after this loop is compiled and linked what exactly is the value we actually don't know we don't know at the time of writing the code right?

only the compiler knows the compiler will finally generate this instruction, telling you what this value is and that's how we achieve our So that's the final result we end up with. The code is already there. Let's give everyone some time now.

Open up that code. Use the environment we set up last time. Let's step through it ourselves in debug mode. and trace through this code. It's very simple. You still remember which make command to use, right? It's make debug, right?

Step through it. As you step through, take a look at what's happening in between. Watch the changes in those registers I just mentioned. That should help deepen your understanding of this. At the same time, pay attention to the addresses it jumps to.

Let's first try it ourselves quickly. Let me demonstrate that for everyone. It's really simple. Let's take a look together. So, this code, it's in this directory. This is very simple, this assembly syntax, we won't go into it again.

Now let's take another look at this debugging process. For example, we run make a debug. After make a debug, here we're actually tracking the changes in x5, x6, x7, the changing states of these three registers, and that's it. Now here we can see,

here is the address of each instruction in the final generated code. Our first instruction actually starts at 0x8000. As for why this value is what it is, we won't worry about that for now. Let's first take a look at So after entering this state,

we know the command for single-step execution is which one? In GDB it's the si command, the 'si' command, short for Single Instruction. So at the very beginning, we first take this x5 register and assign it 0. Now over here, at the moment it first starts executing,

the value of x5 is a non-zero value. Then after running si, we check x5 again, and it has become 0. Right? It has taken effect. Then the next step, is to assign x6 the value 5. So we want to watch the value of x6. At the start it is 0.

Now if we single-step one more time we can see that the value of X6 has become 5. Right? That register is changing there. So next, we need to take X5 and add 1 to its value. So let's watch the value of X5. After executing this step,

what should it become? It should become 1. You can see it's now 1 here. Right? Then the next step, is the comparison. So at this point, we know X5 is 11 and X6 is 5. So are the two values equal? not equal so if they're not equal, we jump

right? what this instruction means is if they're not equal, jump jump to where? jump to loop so where is this loop? according to our code we know where loop actually is loop is returning to the position of the add instruction right?

what is the address of this add instruction? just now we saw the address of the add instruction is 0x8008 this address so if we step one more time it should jump back and continue executing continue with the instruction at 0x8008

you see, the current pc value has already jumped we're back to this x800, right? see? So it keeps going back and forth like this. And then eventually, our values will become different. Eventually, we'll see if we track the value of x5,

it's still 3 right now. Now step again. If we single-step once more, it becomes 4. Alright, step again. Now you can see it has become 5 at this point. So 5 and 5, these two values are now equal. right? And when they're equal, this instruction, as you know,

Will it jump back to this position again? No, it won't. So in that case, under these circumstances, if we single-step through it, you can see it has already continued moving forward, and it starts going to the address 10. Now this instruction here,

it's one of ours below, an infinite loop. This infinite loop is actually there to prevent our program from exiting, right? This is basically the basic usage of this kind of branch instruction. You can go back and look at it together on your own.

OK, so this is Conditional branch instructions. So next, let's look at the next part, which is, this exercise, you'll go back and do it on your own. I won't walk through it for everyone here. So next, what we just covered, they were called conditional

branches, meaning they jump based on evaluating a condition. Now below that, there's another group that are also jump instructions. This group of jump instructions, we call them unconditional jump instructions. That is, it does not judge based on a condition,

so to speak. Now, as for these unconditional branch instructions, there are two in total: one is called JAL, and one is called JALR. Let's take a look at them together. So the first one, is called JAL. JAL stands for Jump and Link.

So why, is there also a concept of Link here? We'll probably see this later, that is, where are these instructions, frequently used? They're used in function calls. We'll look at the specifics later. Now for an instruction like JAL,

its instruction format, is called JType. So now you all know the encoding format we talked about earlier, where does JType come from? The J stands for jump. It means jump. And it, this encoding format, as you can see, there's an opcode,

then an rd, and after that, it holds an immediate value. This immediate value, is roughly, it's about 20 bits, 20 bits. It takes up 20 bits, which is a pretty large range.

It is used for calling subroutines, as we've mentioned before. And then, Now, the key here is how this 20-bit immediate is used. It actually determines the address we jump to, the target address It determines the final target address of our jump.

Now, the calculation method is listed here for you. Please take a careful look and understand this, because it will help you understand the execution process of programs later on. Pay attention to this. It actually takes this immediate value,

which is 20 bits in total, and multiplies this 20-bit-wide immediate by 2 Why multiply by 2? This is the same as before. The reason is the same as before. It means look at this encoding, it goes from 1 to 20, not starting from 0.

Because as we mentioned earlier, since all instructions are definitely at multiples of 2, the address of an instruction is definitely a multiple of 2, so the last bit is always 0. There's no need to encode it, so we just leave it out.

So this, that's why we multiply by 2. After multiplying by 2, it's still sign-extended the same way. So it can store what is actually a signed number in here. This tells us the jump can be a forward or backward jump. It can jump forward, and it can also jump backward.

So what is the jump based on? Actually, it's still the same thing — the jump is based on the PC value. That is to say, the value it finally computes The value it finally computes gets added to the current PC value, and that gives the final result.

This is what's called a PC-based jump. Now, since its range is relatively large, the branch instruction we looked at earlier, it only has about a dozen or so bits, right? A dozen or so bits. I just looked at it and forgot. It's 12 bits.

So this one, over here on this side, there are 20 bits. So 20 bits. It can express a range that's a bit larger. Earlier, it could only jump within a 4K range. This way, it can jump within a range of one megabyte. jump within a range of one megabyte.

So this is how its address is calculated. Now there's one more. It's a little different from the previous command. That is, it has an rd here. So what does rd mean? It means that when this instruction executes, besides jumping

to a new address and continuing to run from there, it also takes the instruction right after this JL that is, that is, that is, suppose this is a JL instruction. Then the JL's suppose this is a JL instruction. Then there might be another

instruction below it. It takes the address of this instruction and writes it into RD. As for what RD actually is, that's up to you to decide. It writes it into there, saved as a return address. So why does it put it in here? Why does it put it in this register?

What exactly is it used for? We'll see that later, and I'll explain it to you. In other words, it takes this Actually, at this point, you can probably already guess what it's used for. Think about it this way: suppose the goal of this instruction

is to jump to a new function to execute. Then after this function finishes executing, don't we need to come back? Think about it, right? When it comes back, what does it do? When it comes back, isn't it going to execute the next instruction?

Think about it, right? So it takes this next instruction address, that is, it saves the address of this instruction in this place, and the purpose is to let this function eventually return back. That's the idea. So we say the purpose of this unconditional jump instruction

is usually to call a subroutine, and that's exactly where the subfunction is reflected. So as for this RD, let me go over what I just said so you can understand it again. Now, in actual programming, it's the same thing—that is,

we also don't write this address directly in the code. We also write a label, and the compiler generates it in the end. I won't explain that here. So this is an instruction called JAL. And it corresponds to this instruction here.

There is also another instruction called JALR. The only difference between these two instructions is that, as we know, this JAL basically means it takes the target address it takes the target address it wants to jump to and encodes it directly inside the instruction.

So the target address is encoded directly in there. As for JALR, it actually is also jump and link, but the R here stands for register. That is to say, it actually takes this target address it wants to jump to and stores it in a register.

And then, it takes the target address out of this register and performs the calculation. Note this carefully. So that's why it's called JALR. This one, so, the difference between it and that JAL instruction, JAL, the difference is,

first of all, its encoding format is not quite the same. It uses the I-type we talked about earlier, which is a kind of... When did we first come across this I-type? It was with that addi instruction that we encountered it, right?

It uses an I-type, not a J-type. And it's also used for calling a subroutine. That goes without saying. So the key point is this: when it makes the jump, how is the target address calculated? Now, as for the calculation method,

take a careful look, everyone. First of all, the RSE here, that actually corresponds to what we call the register. That's the RSE. Since it uses I-type encoding, it also has a field that holds an immediate number. It's just that the range of this immediate number is a bit smaller.

The JAL earlier used 20 bits. Here, part of it is used up by RSE, so it can only use 12 bits here. Then it takes this 12-bit immediate number, and sign-extends it. That doesn't need any further explanation. Sign extension means it can still jump forward or backward.

So the sign-extended value, added to the value in the register, that is, the sum of these two things is the difference from what we saw before. It is based on a register to compute the final address. After adding these values to get the result,

then the lowest bit is set to 0. This is still the same as before, that is, ensuring two-byte alignment. So when it jumps, unlike the previous instruction, which we know jumps forward or backward based on the PC value, here it jumps forward or backward based on the register.

That is where the difference lies. So this is its method for computing the jump target address. Now, similarly, in order to support function calls and returns, it still needs to write the address of the next instruction into rd.

And then when that subroutine returns, it will use the value in this rd to jump back, jumping to the next instruction and continuing execution right? This is the same as JAL. So these two instructions, from the introduction and description I just gave you,

you can see that JAL and JLR are a bit of a mouthful to say, a bit of a mouthful, but just understand what they mean. Jump and link, jump and link. They are both instances specifically designed for our function calls. Keep that in mind.

As for the branch and conditional jump we saw earlier, they are actually designed for writing if-else in our code, for that kind of logic. Think about it, right? So that's all I'll say about the concepts. Next, let's go through some examples

to get a feel for how these instructions are used. Now, for this example, what we've actually provided for you is a piece of code called JALR. Under that directory, let's take a look. The one in here, the example inside JALR, is actually implementing

something equivalent to a piece of our C code, this chunk of code here. Because by default, that is, for this course, we assume you're already familiar with C, but maybe not so familiar with assembly, so we often go the other way around.

First, I'll explain it to you in C, what function we want to implement, and then we'll convert it into assembly. That's the learning approach. Let's take a look at this function. You can see this piece of code. What does it mean? It means that here

we've defined two global variables, right? Now, these two global variables, written out here, means that in the code, we'll generally need to use two registers, right? to store these two values. So the execution of the function starts here,

because we named this function start, which is basically like what we had earlier, that start label we used in so many of our exercises, right? It's essentially the starting position of our instruction execution, the starting address.

Execution begins from here. Now, during this execution, you can see it will go and call a function called sum. So this is calling a function, what we call a function call in C. Now, as for this function call, by now you should know,

it must be using which instruction we learned earlier? It must be either JAL or JAL, right? Think about it, isn't that right? You can use either one, that is, the JAL or the JALR instruction, the Jump and Link instruction. So after calling this function,

We know that as this program executes, this logic will definitely jump to this place, and then execute this. What it actually executes is secondary. So, say it executes something like A plus B. So what's the key here? The key is,

after this function finishes executing, as we know in C, after a function finishes executing, to return from it, what statement do we usually call? We call a return. Think about it, right? So, return— it needs to know now, what does return actually mean?

In the end, this return statement, actually still corresponds to For a JALR instruction, let's take a look at the rough implementation process, what's roughly involved. First, if we call the SUM function, we can actually write it as a single instruction,

called JALX5SUM. Building on what we just described, what did we just say? That is, for the target address you want to jump to, we don't need to write an immediate value. We actually just need to take this label, which is really just a

so-called label, which is just the name of a function, and write it out and hand it over. And once we hand it over, the compiler and the linker will compute the actual target address for us. And then... Take note of this This JAL instruction

has an rd register This rd register as we just mentioned what is it used to hold? It holds for this JAL instruction, the address of the next instruction is placed here So that's why I marked this in red here Basically, you can imagine it this way

Just imagine that after we finish calling the SUM function there is definitely another instruction after it and this instruction here the address of this instruction it will store it into the X5 register So everyone, what is it stored here for?

What is it used for? This brings us back to what we just talked about, this return. Now, this return statement, it actually corresponds to, that is, it is called, we can translate it into, the JALR instruction. As for the JALR instruction,

as we mentioned earlier, what is its purpose? Based on the way it's written here, let's understand, in practice, how does it compute the target address? It uses 0x5. What does 0x5 mean? It means it's based on the value in x5. add an immediate value.

This immediate value is simply 0. So, in that case, the target address it jumps to, is actually equal to x5. Think about it, isn't that right? Because the way it computes the target address, is x5 plus 0, and that's how it gets it.

Now, as you all know, x5, this statement here is executed first. When it executes, it puts a value into x5. What value does it put in? It puts the address of the next instruction over here. Think about it, isn't that right? So when you add 0 here,

the address it actually jumps to, doesn't it end up jumping back over here again? Think about it, everyone. Is that what it means? Can you figure it out? Everyone needs to get this straight. With these two instructions, there's basically no problem.

That is, we use this register, the register x5, and when it jumps over here, it actually does one more thing, namely, it takes the address of its next instruction and saves it into x5. It stores it there. What is it stored there for?

It's for the return. You see what I mean, it's for its use. When it uses it, it uses jr, and based on the value of x5, it computes the return address. And that's the return address. So it jumps back again. So the whole process is this:

jump up and execute, then return and jump back. It makes sense, right? This is how we implement the process of a function call and return. You can see that, right? So everyone, take this code and combine it with what we covered last time,

and go over it yourselves. I mean the code, take a look at it yourself when you get back. Just go through the code in the course material, and make sure you understand the concept I just described. As for the debugging process,

I won't walk you through it, since we already spent a bit of time earlier. Let's finish covering these jump instructions. Let's call it a day. Okay, this code example, we'll just go over it on our own. I won't demonstrate it for you again.

So basically, for unconditional jumps we have two instructions. One is j, and the other is jal, or jump and link register. Two instructions. Saying the full names, seems to flow a bit better. The abbreviations are a bit of a tongue-twister.

Now, here, let's bring up one more concept. That is, as we can see, just now when we introduced the jal instruction to you, as we mentioned earlier, as for its jump range, It's based on the PC up and down that is, that's already a very large range

meaning it can jump within a 1-megabyte range right? But as we know the programs we're writing now might exceed 1 megabyte in size The programs you're writing now if the program you write is fairly small it might compile down to

roughly a few K or a few tens of K which means its range is about that size right? But as your program grows just imagine when you run some large programs nowadays a single executable can get really big It might exceed 1 megabyte.

So at that point, if you try to jump within a range that exceeds 1 megabyte, then simply using an instruction like JAL, well, that's actually going to cause problems. Think about it — wouldn't that be an issue? So what do we do?

The people who designed the assembly instructions have definitely thought this through for us. That is to say, how to handle longer-distance jumps. In terms of data, we generally call this a 'far jump'. So when you look at materials later on,

you'll often see that relative to this 'far jump', there's what we call a 'near jump', which is a short-distance jump. Because it can only jump up or down about 1 megabyte. But if you need to jump beyond that range, that's what's called a long-distance jump.

So how do we implement a long-distance jump? Let's actually think about it this way. We actually talked about this earlier. When we construct a large number, remember what method we used? We introduced this concept called AUIPC, right?

Remember that? By the same logic, we can also, basically bring in the AUIPC instruction, to help us construct a large number, and implement this long-distance jump. On this, let me just briefly mention it. I won't go into the details here.

Just be aware that there's such a concept, and that's enough. Okay. So that's the long-distance jump I just talked about. Now there's another concept, which is that when we do jumps, Actually, there's no need to return. So when there's no need to return,

let's connect this with the example we just discussed. When we returned just now, take a look at this example. What did we actually have to do? We had to take the return address, and save it in a register, right? And then use it when returning.

So now, since we're... Suppose I call a function now, and this function doesn't need to return. That is, the program simply ends. Take this program for example, this is just an exit. Once it exits, it's done. It doesn't execute,

it doesn't continue executing. So what do we do in this case? So actually it's very simple. We just don't use a register to save this return address, and that's it. Think about it, isn't that right? But according to the requirement of this instruction,

here we must fill in a register name. So which register should we use? This brings us to what we talked about earlier. When we designed RISC-V, it has a special register. You still remember, right? It's called x0. This register is actually very special,

right? Reading this register always gives you 0, and writing data into this register is completely meaningless. This actually means this register cannot be used to hold any value for us. So if we want to implement this kind of non-returning jump,

actually, as you can see here, they all use x0. So to make things easier for everyone to write, this way of writing is replaced with some pseudo-instructions. That is to say, as long as it's a J or a JR, then what actually happens is that the rd field is

replaced with x0. That achieves this kind of jump that doesn't return. So this kind of thing is for when you don't need to return, you can use this kind of instruction to implement it. Just something for everyone to be aware of.

Alright, having said that, we've basically covered conditional jump instructions as well. So finally, let me give everyone a brief summary. Up to this point, of all the things we've covered, one concept is the concept of the addressed mode.

So the so-called addressed mode means that In an instruction, one way to locate an operand or its address Now, from everything we've covered so far, across all the instructions, the addressing modes involved — I'll briefly summarize them here for you.

let me give you a quick summary here. The first one is called immediate addressing. It's very simple: the operand is the instruction itself. Here's an example, you probably remember it, addi, which is of course an addition instruction.

With this instruction, the number it operates on is actually given by the instruction itself embedded in this instruction isn't it? the number 20 here isn't it embedded in this instruction? its operand is directly embedded in this instruction

So we call this an immediate operand It doesn't need to it's not stored in some special place it's right there in the instruction This is called an immediate operand Now, there's another type which we call a register operand With a register operand,

the data it operates on is stored in a register and we access the register to get the operand A typical example is the add instruction right? In this add instruction, all the operands are kept in registers, x6 and x7. That's one type.

Now the third type is called base-plus-offset addressing. Base-plus-offset addressing means it works by using what? By using a base plus an offset. So the so-called base, its base address is kept in a register. Then, inside the instruction itself,

you place an immediate value, place an immediate value, and then add that immediate value to the value in the register, which gives you an address. Then, based on that address, you go and find the operand you want to operate on.

That's the third type. This is called register-relative addressing. And then the last kind, which is what we have already seen several times earlier, is that we take a PC value, base it on the PC value, and add an offset to it,

to get the final address. This is called PC-relative addressing. So here, regarding this, the concept of addressing modes, it is really also an important concept in our assembly instructions. So here, let's take this opportunity to summarize this for everyone here,

so you can get familiar with this related knowledge. So, this part about the assembly instructions is basically where we'll leave it for now. Let's take a short break.
