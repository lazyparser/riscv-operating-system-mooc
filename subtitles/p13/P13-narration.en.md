# English Narration Script

Earlier we spent some time talking about assembly instructions — that was the main focus. Now let's move on to the next topic: the calling convention for RISC-V assembly functions. Because when we write code, we spend a huge amount of effort on writing functions.

And calls between functions actually come with some special requirements, which you should be aware of. I'll introduce this in the following parts. Let's take a look. First, let's look at what the function call process actually is.

Let's do a quick review first. So in this function call process, there's a fairly important concept involved, which is the concept of the stack. Right? Now, this concept of the stack, back in the data structures course, we actually touched on it as well.

Let me just briefly mention it here. So what exactly is a stack? When we talk about a stack, there are a few concepts associated with it. First, let's talk about the stack. That is, let's cover the stack first. Stack. When we say stack,

do you all still know what a stack is? What does 'stack' mean in English? Have you ever thought about it? When these foreigners name things, they definitely have a purpose, right? What does 'stack' mean? Stack. Some of you may know.

We say that this stack, in English, is actually, the concept of stacking wood. You know how wood is stacked, right? One log is placed at the bottom, right? Then, you pile another log on top, and keep stacking more and more on top.

Right? Now, how do you take a log back out? You can only take them from the top down. Think about it — isn't that true? You can't pull a log out from the middle, I mean the big logs, like raw timber and so on. You can't pull one out from the bottom.

You can only pull them from the top down. So this stack, what is it? It's a first-in, last-out concept. Right? What goes on first, gets taken out later. So that's the basic idea of a stack. Now, once you understand this, So let's take the stack and

give it a simple definition. For example, we say this stack has a stack bottom, bottom, bottom, and then there's also a stack top, called top. Right? Now the bottom, let's say you take this, just like when we were talking about the wood earlier,

we say this will be the ground later, this is the ground. Let's draw it upside down like this. So the wood, when we stack it upwards, this is the first piece of wood. Then we stack the second piece of wood. When we stack the second piece of wood,

we say this process We call it pushing onto the stack And when we take the second piece of wood out, we call that popping off the stack which is pop, right? Let's do a quick review. So where does this concept of a stack come into play

in our function calls? This brings us to the concept of the function stack. Let's look at the second one first, which is the function stack. Now, for the function stack, we can use this diagram to help everyone get a feel for it.

That is to say, suppose we have a bunch of functions. Functions, as we know, have a certain concept behind them, namely that functions call functions. So let's suppose here we have a concept where function A calls function B and function B calls function C

that's the basic idea Now, when we implement this function call process, we need to define a stack The stack is really just a block of memory And what is this memory used for? What is it used for? It's used to hold, during the function call,

the local variables that are used, or to save some register values, so during the function call, when passing parameters, or handling return values, it saves those values Let's take a look at how the stack works in this process.

So now let's say we are about to start executing from this function A Now, as soon as function A starts executing, every time a function is about to execute, we need to allocate a piece of space for that function to store its own

stack frame the stack frame. So we first need to push a piece of wood onto it This piece of wood is pushed on as a wooden block, and this piece we push is the stack frame of function A. Now, what goes inside this stack frame? So what does it hold? As we know, it holds

As you know, it holds some of the things we define, for example, in the function A we defined, its local variables. In the C language, what do you call these? Remember, right? It's called an automatic variable. Why is it called an automatic variable?

Did the teacher explain this to you before? Why is it called an automatic variable? Well, this variable, this chunk of memory, its allocation and release don't need to be maintained by you, the programmer. It gets reclaimed automatically.

So why does it get reclaimed automatically? It's because it lives on the stack. The stack gets reclaimed automatically. That's the reason. So, this is function A's stack frame. Now function A starts executing. Then the function

say function A executes and executes and executes, and when it reaches this step, it's about to call function B. So if it wants to call function B you know what we have to do we need to allocate another frame for function B right?

And this frame is pushed down through this push-frame approach pushed into this stack region pushed into the stack region Then B starts executing and B in turn calls function C The red color represents the function calls What does a function call mean?

It means the instruction we just saw those jal instructions See what I mean? Then function C starts executing for function C we need to allocate yet another frame So this frame as you can see on the left, the frames of the function stack

keep growing downward So as you can see, I happen to have just this little bit of space left here. Suppose the function calls another function again, what will this function do? It will use up all the space underneath. What do we call this kind of situation?

We call it a stack overflow. That is exactly what a stack overflow is. So when we determine how much stack space to allocate, there is actually some careful thought involved. Then function C begins to execute, and after function C finishes executing,

it needs to return. When it returns, as we know, returning means that within the stack frame of function C, all its automatic variables and such, they are no longer needed. So it has to reclaim them. Now this reclamation process,

is the process of popping the stack. And after we reclaim it, function C gets reclaimed. Then B resumes execution, and function B also returns. So this process, it just happens gradually like this, until finally function A also finishes executing.

And then, at the very end, once function A exits, you can think of it as the program being completely done. After it finishes, all of this stack frame gets released. And then we return to the original starting position. Basically, when we talk about the function call process,

that is exactly how it works. The automatic variables in our C code, they actually live right here. Okay, that's it for this. And let me mention this in passing, for example, in our C code, Where are global variables? Where do global variables go?

Automatic variables go on the stack. Where do global variables go? They go in what we talked about earlier, that segment. Think about it, isn't that right? This is exactly what we covered when we were learning C, learning assembly.

The concepts are actually very close. It's because C is very close to low-level development, and the concepts in C, the concepts in C, many of them actually come from assembly, from machine instructions. Right? Basically, the concept of the function call process,

we'll just review up to here. So that brings us back to our topic today which is why we need to define the concept of a function calling convention Now, as we mentioned earlier with functions, when function A calls function B,

in our terminology, in our specification, we refer to this function A as the caller, called '调用者' in Chinese. In English, it's called the caller. And the function being called, we call it the callee. Right? Since you've all studied English,

you should be able to understand this. The concept of the suffixes ER and EE. So, let's say this function A This caller starts executing instructions one by one, and when it reaches this instruction, it will invoke a call. Now, the call,

as we'll see later, is actually just an instruction. We're simply borrowing it here to represent the invocation of a function. So this function then makes a call. So it calls, and the call starts executing. It starts executing, executing and executing,

and when it gets to this point, we say the call has finished executing. And once it's done, it needs to return. So on return, We know it will definitely return to the caller, to the instruction right after the call, and continue executing from there.

That's the execution process of a function. Now, during this function execution process, what concepts do we know are involved? For example, we say the caller may need to pass arguments to the callee, right? And when the caller

returns, it may need to return some content, which we call the return value. There's also a return address involved, these concepts. So what do all these concepts involve? They involve, that is to say, the interaction between the caller function and the callee.

So it's essentially a there's a boundary here between the two there has to be some kind of where do I put my call arguments that is, where does my caller put them and only then can this callee go and actually get those arguments

think about it, right? When I return, this callee has to put the return value somewhere too and then, the caller also has to know and once the caller knows, it can go and take this data and pull it out So all of these things are really just a kind of

agreement that both sides have to settle on Think about it — if we don't agree on this beforehand, the whole process would fall apart. Now some of you might say, isn't this simple enough? I can put it wherever I want. Like in the example we just had,

we put the function's return address right in x5. Right? You still remember, right? In the example we just had, yes, this very example. In this example, what we actually did was make a convention. What did we do, and how did we agree on it?

We took that return address and agreed that it would be placed in x5, right? So what is this function actually? It's the caller. This is the caller And this sum is what? It's the callee Right? So the two sides actually reach an agreement

We put the return address in X5 That way the flow works on both sides Think about it, doesn't it? So if that's the case why do we even need an agreement? We could just decide on our own If I feel like using X5, that's fine Today I feel like using X5

Tomorrow I'll use S6, and that's fine too Think about it, doesn't it? Right? Well, this is definitely not as simple as you think Why is that? Think about it So where does the problem come from? Well, if the caller and the callee

are both written by you, then of course there's no problem at all. You don't need any convention whatsoever. You don't need an agreement with yourself. Think about it, right? But in most cases, the function you write may be used by someone else,

right? That means the caller and the callee may not be the same person. And that's when you need a convention. You see what I mean? Everyone has to follow one uniform rule for where you put your parameters, where you put your return value,

and where you put your return address. And this way, The function you write is handed over for others to use, so that other people can use it too. Similarly, when the compiler generates machine code, it also has to follow this convention when generating where your

call parameters and return values are placed, and how return parameters are handled. This is a kind of convention. So, as part of our standard, in our assembly, apart from defining these assembly instructions, we also define this part about the calling convention.

This is also part of the specification. As for this whole programming convention, as we will see later, we can briefly summarize it into about three parts. One is the programming convention for registers, that is, how we use these registers.

We have 32 — for example, RISC-V has 32 registers. you can't just use them however you like This is the programming convention for registers. The second one is about the programming convention for function call and return instructions.

That is, when we do function calls, the most critical parts are a function call instruction and a function return instruction. How we should use these two instructions also follows conventions, and there are requirements for them too.

That's the second point. The third point is this: when we implement a function, say I implement a function for others to use, there are also requirements for how that function is implemented. What this function should do, certain aspects of it,

we have specific requirements for. So, once we've grasped these three parts, basically the programming convention part will be clear to us. Let's take a look. First, let's look at part one, which is about the programming conventions for registers.

Let's take a look at this table. This table, that is to say, first, what is the first concept? First, as we know, our registers, in RISC-V, the general-purpose registers, there are 32 of them in total. Now, what did we talk about earlier?

x0, x1, x2 This is one kind of it. In other words, in the spec, the name defined for this register, think of it as a standardized name. You can think of it as its real name. For example, my real name is Wang Chen, right? That's my real name.

This is the real name defined in the spec. But when we program, since we have this concept of convention, because we often use this register, to do a certain thing, we've gotten used to, not using numbers to represent it, because numbers don't convey any meaning.

We give it an alias. This alias, is a name with a certain meaning. Do you get what I mean? Of course, it seems I don't have any aliases yet. This lets me give it a nickname. I can give it an alias. This is called an alias. And this is called the programming name.

That is, when we're programming, we actually habitually write this name. That is to say, in your code, for example, this one, this X0 register, it has an alias called zero. So when you write code, writing either of these two — the assembler recognizes both.

Everyone gets what I mean, right? It recognizes both. But, more often, perhaps, sometimes when you look at code written by other people, more people prefer to use this name zero, simply because it has a specific meaning in this context.

So why does x0 express a certain meaning? When you read it, it's always 0. Writing into it is completely meaningless. You see what I mean. So we give it a real common name, calling it zero, to represent it. That's the idea. When we write programs,

I actually recommend using these programming names, because it's easier to understand that way. Writing those numbers instead is harder to follow. Keep that in mind. So these are their programming names. As for exactly why they're called these names,

for example, why it's called ra, of course, a lot of them are abbreviations. Later, when we go through the conventions and explain them, we'll get a deeper understanding of why they're named this way. And there's one more thing to understand

That is, who is responsible during a function call for maintaining these registers. So when we go through the introduction, we'll walk you through that as well. Let's first take a look at the first one, X0. We already talked about X0 just now,

so we won't go into it. Because it's very special, we gave it a name, zero. Now, it doesn't really have a specific use during function calls, so we don't consider the issue of freely maintaining this register during function calls for it.

So this NA here means not considered, not relevant, Not available. That's what it means. Let's go over it. Okay. So the next register is a very important one. This register, x1, its alias is ra. We say it's actually used to hold the function's

This wording doesn't seem quite accurate. It's not called storing the return It should be storing the value of the function's return address. Please note this. You can go back and correct this. It should be storing the return value of the function address

the value of the address. Storing the return value of the function address. In English, we call it the return address, which actually expresses the meaning more accurately. So ra is actually the abbreviation for return address.

ra, an abbreviation for return address. It's actually what we just talked about when that function returns, the next address it is going to execute, the address specified as the next one. Do you still get what I mean? So, why do we call it caller here?

It means that, it says that, it is the person who calls the function, who maintains this content of this register. So what does that mean? It means that, when, say, function A calls function B, this function B is the callee. Think about it, isn't that right?

That just shows that the callee may, inside its own function, change the value of this ra. So this callee, this function A, will need to, say if sometimes it might possibly, this callee might want to use this value, then you'll have to figure out a way

to save this value yourself. So, we define it as the callee being responsible for maintaining the value in this register. We'll come back to this concept when we talk about the assembly code and look at it again then. You'll probably understand it more deeply by then.

For now, let's just remember this conclusion. Alright, so the second one is called x2. As for x2, we call it sp. What does SP mean? It stands for Stack Pointer. That's the stack pointer we talked about earlier. When we push onto and pop off the stack,

we're essentially moving this stack pointer. So we often use this register to hold the stack pointer. That's why, we named it SP. Now, who is responsible for maintaining it? We say it's the callee function, meaning that inside the called sub-function,

we maintain this stack. Because as we know, popping and pushing are actually done inside the sub-function. In a moment, again through examples, we'll gain a deeper understanding of this concept. Next up, there's a group of registers,

namely x5 to x7. from x8 to x31 how many registers is that in total? 5 to 7 5, 6, 7, that's 3 of them 28, 29, 30, 31 that's 4 of them 4 plus 3 gives us 7 in total these 7 registers we call them temporary registers so the ones starting with t

so the registers here their alias is t t followed by a number now this t we say it actually is you can think of it this way these temporary registers temporary relative to whom? they are temporary relative to the caller That is to say, for these registers,

in this calling convention, when function A calls function B, for example, when it calls function B, inside function B, it's basically our convention that in this function B, you're allowed to freely use these temporary registers.

Now, suppose this function A also uses these t registers, because once it enters this function, these registers might get overwritten. You see what I mean, right? So that means, if my function A uses these registers internally,

then before you call the function, you need to take these registers yourself, and before you make the call as the caller, you need to save their values. And then, when this function returns, You just need to restore them yourself and that's it.

That is to say, these temporary registers, in calls, in these functions, will get overwritten. That's what's meant by temporary registers. So what does this so-called saving mean, think about it. Where do we save them to? Where else

can we use to hold register values? Well, of course they're still saved in the function stack. Right? Saved in whose function stack? Well, of course they're saved by the caller, since the caller does the saving, then the caller saves them in its own stack.

So that's the idea of temporary registers. Next, we have what are called saved registers. As for saved registers, It's the opposite of that one It's called S saved Saved means These all start with S So for these registers it's the caller who is responsible for maintaining them

So what that means is when you call say this function A again, let's say A calls B A calls B Suppose A uses this register S0 Let me give an example Say it uses the register S0 So when A calls B actually, B can always be guaranteed that

when B returns the value in S0 is unchanged That's what we call a saved register So, since everyone thinks if B wants to guarantee that the value in S0 stays the same before and after the call, then one approach is this: you just never use this S0 register inside B.

Does that sound right? But that's impossible, because you might still need to use this S0 register. There are only so many registers— thirty-something might not be enough. So if you do need to use it, what can you do? What does B have to do?

Since it's the one responsible for preserving it, then you find a way to, before using it, save this S0 register onto the stack first, and then start using S0. After you're done with it, before returning, there's one more thing you have to do—what is it?

You restore this S0 back from the stack. Think about it—does that make sense? restore it. So after it's restored, when we return, from A's perspective, from A's point of view, the value of s0 hasn't changed at all, right? Think about it — isn't that the idea?

This is what's called So for the s registers, it's the callee that guarantees their values are preserved. So make sure you understand this, that's the concept. Compared with the t registers, it's basically the opposite arrangement.

Then there are also the argument registers, the argument registers, meaning the arguments. So these registers, they all start with a, they're used when we pass parameters, when calling a function, we need to pass parameters. So for RISC-V,

it recommends that you just pass the parameters directly in the registers, passing them through the registers because that's the most efficient, the fastest way. So that's why we use a0 through a7, which adds up to 8 registers you can use.

In other words, you can fully make use of 8 registers to pass parameters. Then when returning, the return value can also be placed into the two registers a0 and a1. That's what it means. Now similarly, since these registers are used for passing parameters,

that means for the caller, Actually, the value in this register might get changed. So if you want to use it again after using it once, you have to maintain it yourself. So it is maintained by the caller. Now, through this convention,

what advantage can you see? I'm essentially saying that some registers are volatile before the call, they can change, while some registers stay unchanged after the call, right? So the point is to achieve what kind of effect? If you didn't have this rule,

to guarantee that this register is preserved during use, always and never get disturbed one of the dumbest approaches would be this: when function A calls function B, I save every single register, all of them, right? Of course, whether that's the way...

and then, and then, when exiting the function, I restore all those registers back out again. Actually, this can also achieve the same effect. But as you all know, the efficiency is far too low, because we have so many registers. If every time you call a function,

you save all the registers... and restore them again on the way out that is way too inefficient Think about it, isn't it So we simply settle on a convention that is, there is a set of registers that you don't have to worry about

and some registers that you must handle And that way we improve the efficiency of function calls as much as possible Similarly with this standard in place we also reached what we talked about earlier we solved the problem we mentioned earlier

namely, the generality of functions Because this way as long as everyone follows this convention a function of yours whether you wrote it or I wrote it can be handed over to anyone to use So if we all follow this convention, things won't get messy.

So this register convention, that's basically the idea. And now you also understand why these names start with a, why some start with s and some with t. They actually carry meaning. Later, when we write code, we'll try to use these programming names as much as possible.

They are also called ABI names. As for the ABI, the concept, just get a general understanding of it. I won't go into it in detail here. Alright, so first of all, the first thing we mentioned is, the first aspect of the convention is the register convention.

The second aspect, is what happens when we talk about function calls. When it comes to function jumps and returns, in terms of instruction encoding, actually, combining this with what we just talked about, the convention on these registers,

we can do the same thing here, and define part of the convention for its call instructions as well. Let's take a look at what this convention is. Actually, we've already covered some of it earlier. For example, as you can see,

this actually involves some pseudo-instructions. Because pseudo-instructions were also proposed for the sake of making things more convenient for us to use. You see, instructions like this JAR instruction, it is actually a pseudo-instruction.

It actually uses the register convention we defined earlier. Didn't we just agree that x1 Didn't we just agree on X1? That's what we call ra, right? Think about it, isn't that so? So we actually agreed on X1 as the convention to save the return address

and we'll never just use it casually again Unlike before, when we used it however we liked Before we had this convention, we used X5, right? In that example. Now, what I'm saying is, since we've all agreed to use X1, in our code,

we shouldn't keep writing X1 explicitly every single time. So we defined a pseudo-instruction, the ret instruction. For this instruction, notice that it doesn't specify an rd here. If rd isn't specified, So what's actually implied

is the use of x1. So from now on, when we code, when we write our programs, we should try to use the call instruction, which by convention uses x1 to save the return address. That's one point. The second one is this: as we just mentioned,

this kind is called a non-returning function call. That is, as we just agreed, x0 is zero, a special address, a special register. So as long as you put the return address in there, it can't actually hold any return address at all.

So since that's the case, from now on we'll just not write it out so long and wordy. We'll just write a J. JR just represents this kind of equivalent effect. That's the second type. Then there's also a third type, which is — didn't we just say

we have some long jumps? You remember, right? So for long jumps, because long jumps, long jumps mainly use these instructions. And because a long jump itself also implicitly uses X1, or rather, the tail call. A tail call — you can imagine

what a tail call is. A tail call is, in English, called a tail Say function A calls function B, and function B calls function C. Then this function C doesn't call any function. So in the end, this step where B calls C, we call that a tail call.

You get what I mean, right? In English, it's usually called tail — basically the last call. Because this function C can never go on to call function D. You get what I mean. So this kind is called a tail call. Now what about A calling B?

That's not a tail call, because after it calls B, B will still go on to call C. That kind of thing. So that step is not called a tail call. That's called a regular call. So that's what a tail call means. A pseudo call means that

it can no longer save this because after B calls C, C will just C will just Right, it won't go on to call D anymore. So we don't need to save this final return address again. So here we can just use x0. Okay, that's another group of

commonly used ones. Now the last one concerns returning from a function. Because we also agreed that x1 holds this return address, so we don't need to write it out in such a long-winded way. We can just use one Simply put, the pseudo-instruction

ret is the return instruction, used to indicate a return. Of course, this is not the return of the C language. This is the return pseudo-instruction in assembly. Now, this kind of thing involves function jumps, and it's a programming convention.

That is, from now on, we should try our best to use this set of instructions to write function calls and returns. Just so you're aware of this. Now, the ones we use most often are call and return, which are used a lot. That's it.

Now, regarding this thing, let's take another look at the effect. We've made a simple animated diagram for you. Let's take a look at how this works. For example, suppose we now have the caller on the left and the callee on the right.

Let's look at this function process, and take another look at this. That is to say, once this programming convention is in place, the function's invocation how does the function call actually take place? Suppose our caller starts executing instruction i,

then moves down to i+1, the second instruction, and then further down, this instruction starts making a call. Now we know that this call already has a specific meaning. According to our programming convention, what is it equivalent to?

Think back to what we just covered, it is actually equivalent to a long jump. As for this long jump, we don't care about AUIPC. What we mainly care about is the second instruction below, which is JLR. As for JLR, its target, its return address is,

by convention, saved in x1. Think about it, isn't that right? Now, x1, as we said, has an alias called ra. So what it does inside is, what operation does it perform? It takes its next address, that is, the address of this instruction, ra + m,

and saves it in x1. We've now agreed on this: it goes in x1. Don't put it in any other register. Then it violates the convention. If that's the case, code like yours may not interoperate when given to others. Please keep that in mind.

So, this way, we take this address of the I plus M instruction and put it into this register. What's stored inside it is exactly the address of the I plus M instruction. Please pay attention to this. Then, as you all know, what happens next?

This call instruction is nothing more than a jump. It just jumps into this call. Think about it, isn't that right? And then inside the call, it starts executing one by one So our PC starts executing from here and then the PC just keeps executing

j plus 1 j plus 2 this is very simple and then at the end we call return now when we write code we just call return to go back no need to write those complicated instructions anymore this instruction what is this instruction equivalent to

it is equivalent to the jr instruction x0 and 0 parenthesis x1 this is also a convention here because the convention is x1 and what does that tell us it jumps based on the value in here It jumps there. You get what I mean, right?

So, based on the calculation method we just used, it actually jumps to the location of this instruction. So it definitely jumps here. Then the PC comes back, and once it returns, the process is complete. Think about it, isn't that right?

So from this example, we can see that from now on, when we write code, we should always use call and these pseudo-instructions to implement it. So why can these pseudo-instructions implement this process? The reason is simply that

they follow our programming convention, our programming convention— the calling convention for function calls. This is the calling convention for function calls. That is the second aspect. Now, the third aspect is this: Ultimately, we need to write a function.

So how should we write this function? When we write this function, there are actually some conventions to follow. Please pay attention to this. Of course, as for this part of the content, when we write C code, we actually don't pay much attention to it.

Why is that? Because this part is actually handled for us by the assembler. Since this follows a standard convention, all assemblers will, at the beginning and end of our code, insert some special instructions for us. These special instructions are actually related to operations like pushing onto and popping off the stack.

You can't see any of this when we write C code. This is the concept of automation. That is, all this 'automatic' stuff doesn't really mean the program executes itself automatically. It's just that these operations you perform are actually some instructions generated for you by the assembler,

inserted into your code. You all get what I mean, right? Let's take a look at this programming convention, what exactly it does. So when we write a function, actually we only need to write the function body itself. This process of insertion,

actually, inserting things at the front and at the back, what exactly does that do? Let's look at this diagram together. So first, at the beginning of the function, our assembler will help us insert some special push instructions

and some special save instructions. What does it do? At the beginning of the function, this prologue, which is also a term from specifications and the literature, Just so everyone gets the idea, what is it trying to express? It's a bit like...

I actually looked this word up, I pronounce it 'prologue'. It's probably called 'prologue'. Not entirely sure that's accurate, but it's kind of like... what is it like? It's like when a play is performed, when the stage opens up,

we say there are some people who come out first to warm up the crowd. You understand what I mean, anyone seen this kind of thing, or whatever? That is, before the main character appears, there will be some extras, running around in front carrying flags.

Get what I mean? Warm up the place first. This kind of thing might also be called a prologue. That is, a kind of... before getting into the main content. The kind of process described earlier Well, this process is what we call the prologue.

So what does it actually do? First, it says it will decrease the value of SP. Do you know what decreasing the value of SP actually means? What exactly is SP? As we mentioned earlier, SP is a register. This register is actually x-something.

Is it x1? It's x2, right? x2 is actually SP. SP is in fact the stack pointer. So when we decrease this stack pointer, think about what that means. Look at the diagram on the right. It's essentially taking this and pushing the stack further down.

Do you follow what I mean? After pushing the stack, It actually sets up You can think of it this way, conceptually It's like, for this function, it sets up a stack frame for the function. So what is this stack frame used for? It's used to save some registers,

and to save and define some local variables. Think about it, isn't that right? So decreasing the value of SP is essentially setting up a stack frame for the function, and then based on which SAVE registers are used in this function,

and how many local variables there are, that much stack space is allocated. Now, let's go back over this statement. What did we mean earlier by SAVE registers? SAVE registers. Let's go back to that table from just now. When you look at this on your own,

refer to this table along with it. SAVE registers What does that mean? By convention, what are save registers? It means the call has to guarantee the values of these registers Put plainly, for the caller, the values in these save registers

before and after the call must be guaranteed to remain unchanged. Now, for the called function, how do you make sure these registers stay the same before and after the call? Well, the only way is to do what? Don't you have to take these save registers

and save them? Right? Whether you use them or not, Of course, if you optimize it better, you can detect it, meaning if it's not used, then don't save it. And if it is used, then you save it. But maybe you don't want to worry about that,

so no matter what, following this convention, you just save these save registers and that's it. Then before coming out, before returning, just restore them again and that's it. Right? So about the save registers, let's get a feel for what this means.

That is, we take the save registers, based on the save registers used in the function. So in principle, according to which ones you used, if you used them, then you save them. If you didn't use them, Actually, you could also skip saving them.

This is also something that can improve efficiency. This is something for everyone to think about and get a feel for. This so-called saving of the save registers, what does saving it mean, exactly? Now, as for local variables,

that is, suppose I define one inside a function, a local variable called a. So where does this a live? Actually, it also goes on the stack. You also have to allocate space for it. Right? Of course, you — earlier on, on the right side,

as you move this down, when laying out this stack frame, it's actually been worked out ahead of time, how much space this needs to allocate. This is at the compilation stage, and it has actually already been computed for you.

First, pull it down. Then we start putting things into it. That's essentially how it's set up. Then, The second step is to save the saved registers onto the stack. We covered this earlier. Third, if the function will still call Pay attention here.

If the function will still call other functions, what does that tell us? It means this function is not a leaf function. It will still call new functions. So since it's going to call new functions, why do we have to save the ra register

as well? This is exactly what we talked about earlier, the usage of the ra register. Let's review it again. The use of this RA register, as for the RA register, how did we put it just now? Who is responsible during a function call

for maintaining these registers? We said it is the caller who maintains them. So the caller here, that is to say, who was the caller in what we just saw? Let me go back again. Too slow. That is to say, if a function also calls other functions,

that is, if this current function, this function, also calls other functions, then isn't this function the caller? Do you still get what I mean? Suppose this function, if it also calls other functions, that is, if this function also calls other functions,

So this function, is it the caller? If it also calls other functions, then this register must be saved. Think about why. Because we know this function, suppose this function is called by another function, then this function A,

suppose function A called this function, then after function A calls function, before it calls this function, it must have put in the ra register, its return address. Think about it, right? Then if this function of mine calls a new function,

calls C, then suppose this is B, suppose this B calls C again, then when calling C, we know that when calling C, it will definitely overwrite the value in the ra register change it to its return address think about it, right? Now, if it modifies this value,

then the next time it returns and wants to go back to A, it won't be able to get back. That's the problem we'd run into. So, given that this is the case, since I've determined that this function, if B is going to call C, then I need to take the original value in ra,

that is, the original return address of A, and do what with it? Save it. Before my function returns, I just restore it, and that's it. That's the idea. So if this function is going to call other functions, we need to take the value of ra,

what's actually stored in here is essentially, the return address value of function A. the value of this A function we save it and then later return it and just restore it, and that's it

So this is basically, before our function actually executes, there's actually a lot of work to be done. And this work is also part of our convention.

Now, next, let's look at the function epilogue. The epilogue is basically the reverse of this process.

A reverse operation. The first step of the reverse operation is to restore what was temporarily saved. Because we saved these things earlier, and since we saved them, we restore them. After restoring them, with respect to the caller of this function,

A, it's as if nothing has changed. Right? And at the same time, if needed, restore the saved ra from the stack. And what we mean by "if needed" is exactly this, that is, if there is a call, if there was no call, then actually nothing gets saved.

If nothing is saved, then there is no need to restore it. So these two things correspond to each other. Then the last step, step three, increase sp. Now, what we mean by increasing sp, you can imagine, is that we are about to pop.

We are going to deallocate the stack frame. Then the call uses ret to return. This ret is exactly what we talked about earlier, a standard function call. And that finally achieves this kind of effect. So this involves That is to say, the called function,

that is, this function of ours, say A calls B, calls a function, then inside this function, actually, around the function body, before the function body and after the function body, there are some standard operations that we need to do.

This part of the work, we normally don't see it, but that doesn't mean it doesn't exist. It's actually done for us by the compiler. But, today, we're going to try writing it ourselves. Suppose you had to implement it — how would you do it?

to deepen your impression of this process. So basically, that covers everything about function calling conventions, I've now introduced it all to you. So let's go through some exercises to see how this actually works in practice.

Next, we'll walk you through a quick look at these two examples here. One is ccleaf, and the other is called ccnested. Now, this one, basically, leaf, means this is a leaf call, that is, a leaf call, the concept of a leaf call. As for nested,

it means a non-leaf call, essentially. Now, ccleaf, that is to say, what exactly is a leaf call? Let's take a look at this example. If we write it in C, That is to say, suppose you look at this start function, it calls this square function,

just to compute a square, a times a. So after it calls this square function, this function won't call anything else. In other words, after this call, this function, the square function, won't call any new functions. Of course, isn't that right?

This kind is called a leaf call, or a pseudo-call. Now let's take a look. Now, following this calling convention, how do we write this? rather than just writing it however we please. And this, also involves what we talked about earlier, passing arguments,

because this function, We said it also takes a number of arguments. So, as for the arguments we just mentioned, which registers do we use to pass them? According to our convention, we first use a0 to pass the first argument, a1 for the second argument,

right? What if that's not enough? We can also use these, a2 through a7, right? So what about a function, returning its return value? That's also according to the convention, that is, the return value is preferably put in a register to be returned,

so we also put it in a0 first. Then we put A1's in. It works like this. If it exceeds that, we might have to use some other technique. For now, let's set that aside. Let's take a look at this code. This one is in our ccleaf. cc is short for call conversion.

Let's first take a look. So we'll just dive right in. How does this reflect the programming conventions we just talked about? First, note that this program executes starting from the start function. And that function Before this function,

first we see we did a few things. Below this code, we first have a piece of logic here. Now, this logic, let me explain it to you. From its comments, we can see what we are actually defining here, this region, this region doesn't actually hold instructions,

what's placed here is actually, you can think of it as a stack area. We actually defined a stack. A stack is just a piece of memory, right everyone? Since we need to make function calls, we must have a stack, which means we must use memory.

This memory here is defined statically What we actually did was make use of this directive that is, a directive in the assembly instructions to tell the compiler, when generating the executable program, to add roughly 10 words for us here

10 words One Dword is 4 bytes so that's 40 bytes in total You can compile this code and then take a look at the last 40 bytes of it which is actually used to hold the stack area The part before it is used for the instructions So before the function is actually executed,

that is, while our start function is executing, this is where it really starts running, before it actually executes below, we first have a step of operation doing what? We're actually preparing a key parameter for the stack That is, when we push and pop on the stack

we're actually operating on a stack pointer the whole time Right? Now, this stack pointer we need to give it an initial value We assign this stack pointer through the la instruction As we talked about earlier, what is la used for?

It's used to load an address load address That is, it takes an address and loads it into the sp register And this address is stack end So what does that mean? It means that this stack end is what we defined below just now, the stack's what,

the stack's end position What does this mean? It means that, we basically, let's say, this stack of ours, we defined a stack area, right, and at the top of this stack area, according to its definition, this side is start stack, the stack we just defined is what,

it's start, right? start, this character is written a bit ugly, over here is the end of the stack, on top is where we put our instructions, this block down here is the stack area, so for this stack area, at the very beginning, we set the sp register,

to point where? It points to stack end, which means this stack of ours at the very beginning Our stack currently grows from low to high. When we drew the diagram just now, the stack was drawn upside down, with low addresses at the top

and high addresses at the bottom. What we actually do is prepare the stack to grow toward the top, so when we push onto the stack, we push it upward. So we set the stack pointer to point at the high-address region of memory from the start.

That's exactly the effect that instruction had just now. Let me explain this to you. That's what stack_end means. Okay, so this instruction here gets the stack pointer ready. It effectively prepares this stack area. Once the stack is ready,

we start executing our instructions. Now, this one is first put the value 3 into the register a0. You can see here, what we're writing now uses the aliases for all the register names, all of them use their ABI names. a0 — what is a0 here?

It's actually the parameter, right? Based on this code earlier, we can see that that's exactly what it means. In other words, we pass this value 3 as an argument — when passing arguments, we put 3 into a0 first. According to the convention,

a0 is used to pass the first argument. And then, once we've set up this 3, once it's in place, we go ahead and call callSquare. So callSquare is essentially calling a function. Now, this function, the function name is actually this function's

starting address, so from here is really where the function begins. So what's happening up front? We take this stack addi sp, sp, -8 The -8 basically means subtraction. Now, as we just said about the stack, high addresses are at the bottom

and low addresses are at the top. Subtracting 8 moves the stack pointer up. Right? And moving it up is basically opening up space on the stack, isn't it? That is in fact pushing onto the stack. Think about it — isn't that right?

After pushing onto the stack, we then save s0 and s1. Why is that? Because as we mentioned earlier, in the code that comes after this, we deliberately use registers like s0 and s1. So for the register s0, As we mentioned earlier,

who is responsible for preserving its invariants? It's the callee that preserves its invariants. So what that means is, with this function, inside this function, it has to preserve these variables, preserve the invariants of these registers.

So first it puts them onto the stack. sw is store word, taking the value in this register and saving it into the stack. We won't belabor this meaning here. Go back and, together with what we covered earlier, all this instruction stuff,

take a look at it yourselves. Basically, this just saves s0. And then below that, it goes and executes this square, doing the multiplication. Because a0 has already been saved. What's in a0 is our parameter. Then, we take a0 and with a move instruction,

we move a0 into s0 first. Then, we perform a multiplication on s0. Here, we use a MUL instruction. We haven't covered the MUL instruction before. It actually belongs to that multiplication extension, the one that does multiplication.

It simply means taking s0 and s0, multiplying the values in the two registers, and saving the result into s1. Very simple. Then, why does the next one... We also need to move this result back into a0. This is where returning comes in.

Follow me? We said that the effect of our square function here is to take the result the value of a times a, and return it. And when we return it, we pass the return value through a0. That's by convention. It's not something we just made up.

By convention, we simply store the result in a0, and that's it. And finally, our function here — this is actually the body of the function. So actually, right here, and before that, It's basically a prolog. And then after that comes the epilog.

So the epilog is the counterpart of the prolog. Now, over here, we don't need the compiler to generate it for us, because over here we are writing assembly directly, without the step of converting C code into assembly. So our assembler,

well, it simply does not take part in building this code. If it were C, we would have the compiler help us build this prolog and epilog. But we write it ourselves. Writing it ourselves, we then just go along with this they correspond to each other

over here we just saved them earlier saved with sw so now we restore them first with lw that is, taking the values in memory and saving them back into our registers right? restoring them back loading them back so after loading them back

what is this step doing? because earlier we pushed the stack down by one stack frame which is subtracting 8 so over here we have to restore the stack that is, add 8 see that? so this way when this function returns you can all step through it

I won't debug this live right here. I'll just walk through it. You can take a look. This stack has been restored to its original value. So we achieved the intended effect. Now, after coming back, after we come back, over here, down below,

we obviously need to execute a return. After the return, let's look at the effect of the return. Because when we did the call, we took the return address, which is actually the address of this instruction, and put it where? Which register was it saved in?

According to the convention, when we do a call, it is saved in the X1 register. It's stored in ra, right? So when we return, the return actually goes based on ra. So we return back, and when we return, we of course return to this position.

So we exit this function, and execution continues from here. That's the whole process. And this process is a standard way of writing code. It's not just written arbitrarily. Why is it written this way? The reason, based on that function call earlier,

has already been explained to you. Now, here, since it's a pseudo call, you can see that during the pseudo call, we actually don't save ra, when pushing to the stack. Because inside this square function, we won't call any new functions again.

Note that, and correspondingly, there's that nested example. Let's take a look. So this function, it goes like this. That is, it, it will, this start function, will call aabb. aabb, in fact, is basically, it implements what? It's a squared plus b squared.

However, this function is not a leaf call, because inside this function, it goes and calls the square function again. The square function is a pseudo-call. What we mainly care about here is how to write this AABB function. In this example, we mainly care about how to write AABB.

It's not a pseudo-call. So let's look at the difference between it and the square function we just saw. Get a feel for the difference in programming convention and writing style between pseudo-call and non-pseudo-call functions.

So we won't go over this square function now, because the way it's written is the same as in the previous example. We'll mainly look at the AABB function. So for the AABB function, let's take a look. First, let's look at this is

the start function calling the aabb function, since it has two parameters, you can see that it actually uses They are saved in a0 and a1. Right? By convention, a0 holds the first argument, and a1 holds the fourth argument. So this call,

it's still the same, it jumps, and then, it starts calling this function, ABB. Now this function, you can see the difference compared to the one over here. Besides saving s0 through s2, what else do you see it saving? ra. Why does it need to save ra here?

Think about it. Based on what we explained earlier, because this function AABB inside it, it will also call square JR of course, here what's used is JR what JR reflects is that it's a pseudo-call it won't come back again so, this function

because it will still call JR as for JR this instruction will go and modify the RA register everyone, pay attention so, in order to guarantee because if we say if you don't save this RA just think about it when this instruction runs

it will have modified this RA register Then it got changed to it got changed to its own address of this instruction Right? Because it is this instruction's the address of the next instruction, right? Now, if you don't save this RA,

relatively speaking, that is, if you don't restore this RA here, then when you do this return, it won't be able to jump back. Think about it, isn't that so? It just won't be able to jump back. Jump back to our the position after

this start function, this problem will occur. So let's think about it ourselves. That is to say, why we save ra here. In other words, the difference between this AABB function and that tail-call function we just looked at, the Square function,

is this: look at this function here, it doesn't need to save ra, while this one does save ra. That's the reason for it. I'll leave this for you to look at on your own back home, so I won't say more about it here. The program is very simple,

what it implements is very simple in itself. The main point is through these two examples, get a feel for it: now that we have these calling conventions, how do we write a standard RISC-V assembly function? This is the content of this chapter

that we want everyone to really master. Now, for these two examples, I won't debug them right here to show you the result. Based on what we just covered in class, especially the calling convention, the use of these registers, the use of function instructions,

and also the callee's prologue and epilogue, and the way the call is written, go debug it yourselves, and trace through the execution of this function, and get familiar with the concepts of the calling convention. As for 5-8, that's also left for you to do on your own at home.
