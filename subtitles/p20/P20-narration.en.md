# English Narration Script

Alright, let's get started. Today we're going to begin Chapter 10. Let me quickly recap: in Chapter 9, we introduced how to implement what's called a cooperative multitasking system, right? A mutual-aid, cooperative multitasking system.

Now, we also mentioned that when it comes to multitasking, aside from this cooperative approach, in modern operating systems, what's actually used more often is preemptive multitasking. Now, since we're going to talk about preemption,

there are some concepts we need to understand first, namely, the knowledge about exceptions, interrupts, and related topics. So today, our entire lecture, all of these chapters, will revolve around implementing a preemptive multitasking system.

So first of all, let's take a look at the topics related to interrupts and exceptions in this type of system So in the upcoming chapter 10 Traps and Exceptions we mainly divide it into these parts First, let me introduce what control flow is

and what a trap is Then, second, I'll explain how in RISC-V handling a trap involves which programming interfaces that is, the use of registers And then we'll look at the trap handling flow in RISC-V I'll roughly cover it in three parts

Related reference materials mainly the standard specifications I won't go into those again Good, let's first take a look at the concepts of control flow and traps. So first, let's understand what control flow is. Control flow,

as we call it in English, is called control flow. Actually, it corresponds to what we'll talk about later, exceptional control flow. That is to say, the so-called control flow is the non-exceptional kind, the normal kind of control flow.

So when we talk about our programming, our programs, the programs we write ourselves, the process of their execution can actually be thought of as a kind of normal control flow, that is, control flow. Of course, in this process,

apart from some arithmetic instructions, Beyond these basic instructions, we also have things like branch instructions. You've studied jump by now, right? That is, conditional branch instructions, as well as unconditional jump instructions.

Now, whether it's an unconditional jump or a conditional jump, as long as it's a control flow that our own program controls itself, it belongs to normal control flow, the normal execution flow. Now, corresponding to this normal control flow,

on the other hand, we have what we call exceptional control flow, called Exceptional Control Flow, abbreviated as ECP. Now, this exceptional control flow, what it actually means is that it is outside the control scope of our program,

and it is mainly divided into two major categories. One category is called exception which is 异常, that is, exceptions The other category is called interrupt which is 中断, that is, interrupts Now, going forward, we're going to learn together

exactly what an exception is, and what an interrupt is. Now, in this chapter, we'll mainly focus on exceptions, that's what we'll mostly talk about. Later on, I'll introduce to you the concepts related to interrupts. Now, in our

RISC-V architecture, this notion of exceptional control flow, this term from the computing world, it has its own name, called trap. Now, as for trap, if you just take the word literally, That is to say, it is a concept of a trap.

The idea of falling down. As for why it's called falling down, and not flying up, or going upward, once we learn the concepts that come later, you will all understand. Let me first introduce a few of these terms to you. So in this RISC-V,

regarding the trap, we now know that a trap covers both exceptions and interrupts. They are all called traps. Collectively called a trap. Now, for this trap, how does it operate the related interface? That is, which registers does it involve?

Let's take a look at that first. Now this here, is a table. And over here as well, I also mentioned to you along the way, that is, we are in Machine mode. Think back for a moment. Earlier, when we were covering the RISC-V instruction set architecture,

we also learned about this. RISC-V has several operating modes, right? You still remember, right? How many are there? There are three, right? Isn't that so? User mode, supervisor mode, and machine mode. Now, in this course, that is, our work,

is mainly focused on machine mode. So in each of these modes, it has its own set of CSRs. Do you still remember what CSRs are? Let's recall that for a moment. Let's review it. CSR stands for Control and Status Register. Right? As opposed to the general-purpose registers,

the general-purpose registers, meaning x0 to x31, those 32 registers. So beyond those, in some privilege modes, that is, machine mode, Supervisor mode, modes like these, each one has its own set of special control registers. Those are the CSRs.

And mainly, since we're introducing machine mode, let's focus on understanding them. the related registers in the machine mode Now, these registers are related to our trap handling in the CHAP, and this table simply lists them.

For more details, you can refer to the privileged instruction set manual, specifically Table 2.4, for the specific registers inside it. Later on, we will introduce them to you separately. So among these, in the earlier calculator example,

there are actually quite a few registers, but, the ones related to trap handling, we have gathered and listed them out right here. For example, mtvec, mepc, and so on and so forth. So this is actually a summary table Now, you guys

may take a look at it right now and it might not be immediately clear what exactly it is for After we finish the later content, we'll come back to it This is actually more or less a summary table so I won't belabor it here Let me just say that

the registers we covered today those registers are mainly the ones listed above These last two when we get to interrupts like MIE, MIP and the like when it comes to interrupt handling, we will cover them in the next lecture and introduce them to you then

So let's first look at the registers we covered earlier. Among them, we introduced to you the first register related to TRAP, the register related to traps, called MTVC. Now all these registers have a leading letter M in their names,

and this M means it belongs to Machine mode. If it were in another mode, it might be replaced, with an S, for example, or something else. So here we see this M, and it stands for Machine mode. Now the TVEC that follows, is actually an abbreviation of English words as well.

The T, is short for Trap, as we call it. And VEC, It's actually Vector. Vector expresses the idea of a vector or an array. We'll come back to that in detail later. So what is this register used to hold? As we can see here, it's mainly used to hold some base values.

So this is the description of the register from the specification. Now, as we mentioned earlier, things like this MXLEN are related to the CPU's word length. It could be 32, or it could be 64. So they use a single red symbol to represent it.

And what this is saying is that the front part of this register is divided into two parts. The latter part has two bits, 01, two bits, used to hold something called MODE. And the remaining part at the front is used to hold this BASE.

What exactly that means, I'll explain to you later. Also, there's one more thing mentioned here, which is that there's also a special term that appears here, called WARL. This is also one of the special things in the specification.

this term in the spec there are a few more later on Now let's briefly talk about WAR What it actually expresses is this It is actually an abbreviation called Write Any Value Reads Legal Value So what it specifically means is this field,

this base field, you can write any value to it but the value you read back is always legal That's what it expresses As for the specific meaning, I won't go into it further here Those of you who are interested can go take a look at section 2.3 of the privileged spec

for the related content about this CS, Now, this is a standard description, and from its wording, it's actually pretty easy to understand. It's basically about this part here. Because this register, it can be both readable and writable,

which means that for the content in this part, you can write to it however you like. And then, to make sure that, it guarantees that what you read out is always legal content. So, sorry about that, I accidentally turned it off earlier.

So let's now mainly look at, that is to say, this, the parts of this register, these two parts respectively, what is it used for So let's first look at this base on the left As for base, it is actually used to hold what it says on it is

called the base value of the trap entry function What does this mean? Well, you can imagine that when an exception occurs, or an interrupt, its main purpose is to make the whole machine, the entire execution flow of the machine,

jump to a special handler function, to execute this special trap handler function. Now, where this trap handler function is placed, this location can actually be customized yourself. So how do we tell the CPU about our base address of this function? Where is it?

In fact, we just need to fill it in here. Then, it says it also says we must ensure 4-byte alignment. That's mainly because as you can see, this register, it actually in total it's let's say if ours is 32-bit, then in total it only has 30 bits of length,

because the last two bits are used for other content, right? So what that actually means is, since it's in other words, if a complete address It should be 32 bits on a 32-bit machine, its full address length should be 32 bits. So if the last two bits

are not used, or are used to hold something else, then by default, the last two bits are both 0. So by default, if the last two bits are 0, then according to binary representation, if the lowest two bits are 0, that means the whole address

must be a multiple of 4. In other words, if what is stored in front is 5, then its actual address is 20, because the way it is expressed here So he expressed it this way. What's stored here is the upper 30 bits of a 32-bit number,

the high 30 bits, and the last two bits are actually 0 by default, so he used them to hold other content. Pay attention to this. This way we can, so to speak, express an address using just 30 bits, and the last two bits, being left empty, can hold other content.

That's where this four-byte alignment comes from. And then, the two bits after that, they're called mode. As for mode, it's defined as follows: it can be used to control how the entry function's address is configured, that is, the address of this entry function,

the way it's stored. It has two modes. One of them uses what's called direct mode, and the other is called vectored mode. Now, as for the values, they're also on this table. If it's direct mode, then the last two bits are 0. If it's vectored mode,

then that means it's just 1. And 1 means this side here holds 01 in binary, right? So specifically, what is direct mode, and what is vectored mode? Let me explain that to you. So, direct mode is described in the text like this: after all exceptions and interrupts occur,

the PC jumps to the address specified by base. So let's draw a diagram to illustrate it. It's actually in this form. That is, you can imagine in memory, we take this exception handler, which we call the interrupt handler here,

strictly speaking, it should be a trap handler. So this trap handler, this function is placed in the memory region starting at the ebase address. And then we tell this address through this register to our CPU, and at the same time fill in this mode value as 0.

Then when a trap occurs on the CPU, it will jump to the location at the base address, and start executing this trap handler. So as you know, this base direct method, this base direct method It means this handler function is only one.

We just need to give it an address, and that's it. So as we all know, there are many kinds of exceptions and interrupts. For different exception handlers, we might need to provide different handler functions. So how can we, within a single handler function,

handle multiple exceptions and interrupts? Well, you can imagine, that inside this interrupt handler function, if we were to write it in C, then there must be a switch-case type of logic in here, right? And then, based on the different exceptions and interrupts,

which we'll talk about later, the CPU will use other means to tell us, when this interrupt occurs. When this trap occurs, what interrupt it is, and/or what exception it is, then we can go through this kind of switch-case to determine,

and then in this handler function, take the branch. This is what's called the direct method. Now, corresponding to it, there's what's called the vector method. What do we mean by the vector method? Actually, it's very simple. That is, it doesn't provide

just a single function; it provides multiple functions. And at this base address, what's actually stored is an array. You can also understand it this way: that is, I can have in memory an array defined This array, each entry is 4 bytes,

and each one actually holds an address. So it's a pointer array. We take the base address of this pointer array, as the base, and fill it into this register, and then tell the CPU. When a trap happens, the CPU, it will jump to this base location,

jump to this base location. And then, based on this, the specific kind of trap, exactly what it is, we'll talk about this later, that is, for different exceptions and interrupts, there is actually a code, which is really just an index value.

Based on this index value, it is then used as an index into this array. And then based on this, you can find the corresponding entry in the vector table where this exception lives. So this is exactly the idea of a vector. Now, each entry in here

is really just an address. Suppose I get this exception number, say it is 2, then based on this base address, it offsets by one, offsets by two, 2 times 4 bytes, right? If it is 3, then it offsets by 3 times 4 bytes. So say it is 2,

then for 2 it offsets by this much, and after offsetting this much, it arrives at this position. right?... So after we get to this location, what's stored there is another address. And this address, in fact, points to a handler function that actually corresponds to it.

In other words, it actually means it replaces the approach where, in our code, we write switch-case statements ourselves and implements it as a pointer table style. Now think about it, these two approaches definitely have definitely have a purpose.

Why is one called the direct mode, and the other called the vector mode? Think about why. Well, it's actually pretty easy to see, That is to say, if we use the vector method, if we use the approach I'm using now, compared to the earlier version where we formally write a switch case,

its handling efficiency is definitely a bit higher. So when, if in our system, the interrupt or exception frequency is relatively high, and we frequently jump into this kind of interrupt and exception handler to execute, we actually recommend that you use this vector method

to configure its entry address. In normal situations, we just use the simple base method, and that's actually enough. So the vector method is more suited to this kind of interrupt handling where faster response is required, and where interrupts occur frequently,

and when higher efficiency is needed. We configure it this way, and that's it. The effect is actually the same. Now this one, is a register called MTVEC, which stands for Machine Trap Vector Base Address. That's what this register is.

Very simple, it's used to configure the entry address of the trap handler. That's the first register. Now the second register, is called MEPC. What it means, is a kind of exception program counter in machine mode. Program counter,

we've mentioned it many times before, it actually represents The execution of this program, the concept of the current position — right? So it is actually also used to hold this kind of information about the program's execution position.

So its length is, for example, right now we are only using 32-bit, so it is 32 bits long. So when exactly is this register used? It is described very clearly here: that is, when a trap occurs, we know our program will definitely jump to

that mtvec, the register we just mentioned, and execute at the address it holds. Now, as for jumping to that address to execute, well, everyone can imagine, So what actually happens is that our normal PC the PC we keep mentioning earlier

the value inside it actually gets replaced So this is what the so-called jump refers to that's really what this sentence means Now, since this PC has been replaced we know that exceptions and interrupts actually express a very fundamental concept

We may have gone a bit too fast just now and skipped over it in passing It's this figure from earlier this figure right here that is to say, whether it's an exception or an interrupt it actually follows this same basic idea right here

meaning that when our normal these two lines actually represent our normal control flow and this is our trap handler this is the exception control flow So what this dotted line expresses is that when, say, an interrupt or an exception occurs

our execution flow will jump from the normal control flow to this exception control flow to execute a special handler function but after the handling is done, it's actually not finished it still has to come back see what I mean?

it still has to come back so this jumping out and coming back, this process is a bit like our function call execution but, as you know, this call and execution is not something that happens under our control it's a hardware handling behavior of the CPU

so since it has to jump out, when it jumps out, as we just said, when it jumps out, Our PC value will actually be replaced Then, when we come back, for it to continue executing, this PC value must be restored Think about it, right?

It must be restored And since it needs to be restored, that means at the time of the jump, this original PC value because we've already jumped to here, the PC value gets replaced with the value at this location Right? So this PC value has to be saved

And saved where? Actually, it's saved into what we just mentioned, this thing called EPC which is specifically used for exceptions, used to save, when an exception occurs, at the moment it occurs, the PC value of that exception

So when it comes back, we restore the values that were in here, and then it can continue executing downward. There is actually a game like this, which maybe just now, when we were talking about this register, was mentioned briefly in passing.

Let me go over it for you again. Okay. So let's go back to this MEPC we were just talking about. So now everyone knows, this MEPC, this register, is actually used to save when a trap occurs, since this PC gets replaced, we have to take that

original PC value and save it. Where do we save it? It gets saved into this MEPC register. So what we just mentioned That is, exiting this CHAP. So after the CHAP processing is done, we have to come back. When we come back, how do we make that PC value

get restored back to the MEPC value we saved? Well, we'll talk about that later. It uses a special instruction, called Mret here. Ret just means return. This one, we'll talk about it again later. It's not quite the same as the return instruction

that we normally call. It's a special instruction used when returning from an exception back to normal execution. So when this instruction is called, it actually makes use of the MEPC that we just talked about. So it will take that original

value and restore it into PC, which achieves the effect of returning. Combine this with that diagram from earlier and take a moment to understand it again. This is the effect of mepc, the effect that mepc is meant to achieve.

Now, on this point, I should also mention here on my end, namely that this restoration, that is, at the time of saving, what mepc saves might be the current instruction, or it might be the address of the next instruction. As for when the current address gets saved,

and when the next address gets saved, we will cover that with you later on. But in any case, to sum up, it is always used to save a value related to the PC Now, what happens next? When we return, the value is restored into the PC,

achieving the effect of a return. So in the normal handling process, it's mentioned here that its value can also be modified to change its return address. In our later examples, we'll also cover this kind of usage. So when we get to it,

we'll look at it then. For now, here, just knowing that this exists is enough. This is what we need to understand, that is, the second register related to exception handling, called MEPC. Now, the third one we need to understand

It's called Machine Cause. Cause means the reason. Because, because, right? It means the reason. Now, as we mentioned earlier, when a trap occurs, just like that one earlier, when we were talking about mtvec, we also mentioned that

the CPU will record the cause of this exception — specifically, what kind of exception it is, what kind of interrupt it is — and tell us through this register. So we can read the value in this register and just do a switch case,

right? Taking different branches. So this is used to tell us and store the exception code. The so-called exception code, that is, the register holding the cause value. Now, one thing mentioned here is that on this side they have written another one,

the spec says that this field, its attribute is called WRL. Now, as for WRL, what they write here is it stands for write read only legal values. So what it really means is that when we read or write this field, it must ensure that its value

for example, when you write to it, its value must be legal. If it is an illegal value, meaning one not defined in our spec, or some value it does not recognize, it may trigger a further exception. Please keep this in mind. This is the meaning of WRL.

Let's talk about it briefly. So this this register, that is, when an exception occurs, this Hart, that is, the CPU, it will set this register, to tell us about this trap, the specific reason it happened. The specific reason is actually a value,

this value. And then when it encodes it, that is, the whole thing, for example, if it's 32 bits, its most significant bit, the most significant bit is used to tell us whether it is an exception or an interrupt, because as we mentioned earlier,

traps fall into two categories. It's either an interrupt or an exception. So here, if this highest bit that is, the highest bit of this 32-bit field is 1, it means the trap that just occurred is an interrupt. Otherwise, if it is 0,

otherwise it's 0, and 0 means it's an exception. So by looking at this highest field, we can quickly tell apart whether what just happened is an interrupt or an exception. Now, what about the remaining part? As we just said, it's the specific

which interrupt it actually is, or which exception it actually is, the specific value. For this value, the specification also has a table that defines You can take a look at this table in the privileged spec, section 3.6, Table 3.6.

Now, as you can see from this table, what it actually means is that this highest bit here, meaning that this side is all ones, right? If it's all ones, then that means this side is this category is interrupts. And if the highest bit of this category is 0,

then they are all exceptions. And it also defines, for instance, these 0, 1, 2, 3, 4, 5 respectively. When the front part is an interrupt, what do 0, 1, 2, 3, 4, 5 each mean? And when it's an exception, what exactly do the exception values 0, 1, 2, 3, 4, 5, 6, 7 mean?

Now, for us, Today we're mainly going to talk about exceptions. So for interrupts, we'll leave them aside for now. As we can see, exceptions are divided into many kinds. For example, instruction address misaligned, instruction access fault,

that is, the access failed, and illegal instruction, and so on. In other words, when the CPU detects that our program has triggered one of these problems, it will raise an exception, and notify us. It then jumps to the specific exception handler

logic to execute. That's roughly what it means. This is the third register, which is mcause. Then there is also one called mtval. What this actually means is that it assists that mcause register. Because mcause is just a single value that tells us specifically about this exception—

for instance, what kind of exception it is. For example, what kind of exception it is. Now, when certain exceptions occur, it may also need to give us more detailed information. So that single register from earlier might not be enough.

So this mtval register, together with mcause, acts as a supplementary use of mcause— an additional description. Through it, the CPU can that is, the CPU can use this register, along with the mcause from before, to give us more detailed

information about the exception. Just get a rough idea of this, and I won't say more about it. Now there is another register, which is also quite important, called mstatus. It is used to describe some status information. It describes some status information.

So let's look at it, let's take a look. That is to say, the whole thing is 32 bits, and it is divided very finely. Every bit has a specific meaning. Among them, here another special one appears. This term is called WPRI. So what is written here is,

WPRI means, Write Positive, that is, on writes it keeps the reserved value. When reading, it is ignored. So in fact, that is to say, for this part, it actually does not, the specification does not actually use it. That is to say,

when it comes to writing for us programmers, actually, what it really means is when we operate on this timer, then this part, as much as possible, don't write to it and if there is a write operation, try not to touch it whatever value was there originally,

preserve it so the real idea is this: when we write to this timer, first read out the timer value, then modify just the bits you need, and then write it back, and that's it so this means when you write, you must ensure the values inside stay unchanged,

don't disturb the original values in there and when reading, The values read out of here are meaningless to you right now, so you can just ignore them for the moment. This part is actually reserved by the specification, so let's just leave it alone.

As for the rest, the parts that are already defined, those are what we can read and write. Now, among them, it splits this into many fields. We'll just pick out the ones we're dealing with and walk through them for you. The parts that aren't highlighted in that color

also have specific meanings, but since we're not using them, we won't talk about them here. Now this one, for instance, let's first look at the red part. That is, these lowest bits, namely bit 0, bit 1, and bit 3. So it actually follows a very regular pattern.

Take a look, everyone. That is, if you look at the naming, it's all like this: You see, they all come in these groups. Group by group. For example, over here they are all IEI. Things like IEIE. And it's only the front part where they swap in a USM.

Now, you can probably guess what USM stands for. It's actually what we've been talking about — these privilege modes: the user mode, the super mode, and the machine mode. They all follow this pattern. MSU. So when we discuss them here,

we just use an X in front as a placeholder. Just keep that in mind. It's divided into xIE, xPIE, and xPP That's what it means So these specific three fields are split into three categories What do these three categories actually represent?

Let's look at IE first As for IE, it's actually an English abbreviation which stands for interrupt enable that is, the interrupt enable bit meaning that this these three bits actually control our under different modes, whether the global interrupt

should be enabled or disabled So when we want to for example, when we want to take the interrupts in machine mode and enable them, we set this bit That is, at this MRE field, just set it to 1, and that expresses this meaning. If you want to disable it,

just set it to 0. At the same time, we can also, by reading one of these fields, know whether the current interrupt is enabled or disabled. That is to say, when writing, you are setting the interrupt, disabling or enabling it. And when reading,

you can find out whether this interrupt is currently enabled or disabled. This is XRE. Now, what about XPRE? This P here, it is connected with this RE that follows it. Also the concept of interrupt enable, and this P actually stands for 'previous'.

It means previous, as in the state of the previous interrupt enable bit. What it's saying here is essentially that when a trap occurs, it is used to save the xRE value from before the trap occurred. In other words, suppose this,

for instance, at the very beginning, the interrupt is enabled. Say the value here was originally enabled, say it was 1. Then, when we are in machine mode and an interrupt or an exception occurs, it will take the value in here and first save it into this one,

save it into this one. Then, once the exception has been handled, it will use the value in this one and then perform the corresponding restoration. It has this kind of use built into it. This is XPI. Then XPP, as for the P, this P actually,

in English it should be, the first P is still previous, meaning the previous one. As for the second P, it's privilege, which means access rights. So it's previous privilege. Now, for this one, it's also written right here, it is used to store

the privilege level from before this trap occurred, the previous privilege level from before, this privilege value. In other words, So, for example, when a trap occurs, then we know we need to bring this up here, and that is, when a trap occurs,

what happens on this side is, let me explain this to everyone, the question is, why is there no UPP? Because, as you can see, there are only SPP and MPP here, meaning there is only the previous privilege in machine mode and the

previous privilege in supervisor mode, but no UPP. Now, let me explain this to everyone, that is, we say that when an exception occurs, there is actually a concept behind it, namely, it always goes from this That is to say, when an exception occurs,

its overall privilege, this level, it always goes from a higher, from a lower privilege level to a higher privilege level, and never the other way around. To give you an example, let's say, you are currently working in user mode,

then while in this user mode, say an exception is triggered, for instance, you execute an illegal instruction, or an interrupt comes in, then at that moment, the CPU, it will jump from user mode into M mode. We'll cover this later on,

it always jumps into M mode, but it won't just When an interrupt occurs in M-mode, it jumps back. That is impossible. That's impossible. So think about it, since it is this kind of since it has this kind of effect, then it's a downward

you can think of it as the top is the one above is below so since this goes downward, it's the concept of falling into a trap. So this is what a trap means here. That's where this meaning comes from. trap literally means a pitfall.

It means falling down. So this always goes from a lower privilege level to a higher privilege level. Because usually when we draw diagrams, we always put say, kernel mode always at the bottom, and user mode at the top. So it is always a

switch from top to bottom, a process like that. That is why this is called a trap. Now, maybe having reached this point, you may wonder why it is called a trap, and that is exactly where the name comes from. So this way, we can go a step further to understand.

That is to say, as we just mentioned, it can only be from U to M, so trapping into this M mode How many possibilities are there? One is U to M. It can also be S to M. And then there's another one, M to M. That means it was already running in M mode,

and then an interrupt occurs, so it still enters M mode. So for this M MPP, there are three possibilities. And since there are three possibilities, we need at least two bits to represent them, right? Because two bits can represent up to four cases.

Since we have three cases to represent, that's why this field needs two bits. Why do we use two bits to represent it? That's exactly the reason. Now, for S mode, does anyone know when we would trap into S mode? So there are only two possibilities.

Either it switched over from U, or S switched over by itself. Right? We say it can't have come from M, because you can't go from high to low. So, since there are only two possibilities, we only need a single bit to represent them here.

0 and 1 can represent them, right? Because look here, U is 0, and S is 1. So for S, there are only its previous mode, the type of the previous mode before the switch, there can only be two. It's either U, or it's S, right? It can't be M.

Now as for U, since we said As for its trap, it can only trap into itself. So since that's the case, there's actually no need for us to use a dedicated bit to represent its previous one, because the one before it can only be itself.

Think about it — isn't that right? So we save where we can. When designing this register, we have to be frugal, because every bit is very precious. So as you can see, this UPP is not needed, and that's the reason why. Moving on to here,

it actually also involves this TRAP — what kind of process it really is. In that case, as we know, this mstatus register, It actually contains a lot of information. It can be used to control whether interrupts are enabled. It can also indicate some status of the interrupt.

As well as the interrupt status before the interrupt occurred. As well as, before the interrupt or exception occurred, the privilege level information. We will use these later on. So, this register is also very important. This is mstatus.

So, for the design of the registers, we'll just stop our brief introduction here. Next, let's take a look at the trap handling flow. The trap handling flow. As for the trap handling flow, it's actually quite simple. That is, first of all,

we need to initialize it. Initialize The initialization process actually includes for example, setting up its entry address and other related concepts. Then, the second stage is what we call the top half, the upper part of the trap.

Now, the top half of the trap is essentially saying that, the actual process of it happening takes place inside the hardware. It is a processing flow inside the hardware. It is not something we control. It is a hardware processing flow.

So after this hardware processing flow is completed, only then does it jump to our software part. This is what we call The bottom half of this trap, so the bottom half of this trap, this part in blue, you can think of it as a piece of

program logic that we wrote. Whereas the top half just now, actually happens inside the hardware CPU, so it is not under our control. Then this bottom half part, that is the logic we execute. After that execution is done, please note that,

there is one last thing, that is, there must be a return. Because as we have emphasized many times before, in the execution process of this TRAP, it has a return step, that is, it returns to the original normal control flow. So its whole processing flow,

we can simply summarize it as These four major steps So next, let's take a look and look in detail at what happens in the process of these four major steps what exactly we need to do or, in other words, what actually happens Let's start with the first one

which is trap initialization Now, as for the trap initialization, it's actually very simple Combined with what we just covered about these registers, the first step is actually to set the entry function which is our bottom half code,

its starting address because this address needs to be told to our CPU, so that when a trap occurs on this chip, it will only then jump to this address to execute the second half of our handler. Does everyone understand that? So we need to tell it

this address, and this address is what we call the base address. So this base address, how do we tell it to the hardware? We actually need to set the register I just introduced to you, the mtvec register. Now this is something I extracted from the code.

This is actually a it's a function, it's a function. How exactly it is operated inside this function, we'll go look at it ourselves. I won't go into that again. That is to say, what this actually means is we need to write this, write the MTVC register,

and set inside this register our base address of this entry address, that is, the base address. Now, here you can see this entry address, this function is actually a piece of assembly logic that we wrote in entry.s, the assembly file.

Now, this assembly logic, as we mentioned before, in the syntax of this assembly, this label is actually this, it's actually just an address. Think about it, isn't that right? This label, in fact, in terms of the concept of relocatable linking,

It is actually just an address. So we take this label, fill the label into this place, which is essentially putting the address of this function into this calculator. That's its initialization. Very simple. Now for the second part,

we say it's the top half. The top half, this part is actually done by the hardware. Our software part is not involved here. But we need to understand what this part is doing, so that we can better understand what our bottom half should do.

So let's first take a look at what this top half does. So for the top half, we say the hardware automatically performs the following state transitions, meaning this is what the CPU does. So let's look at what it does. Its first step

is that it copies the MIE value of mstatus into MPIE. Now, what does that mean? We mentioned earlier that the concept of MIE is that when this trap occurs, the state of our interrupt enable bit tells us whether interrupts are currently on or off.

It copies that value into here. Copying it must be because it will be used later. Let's set that aside for now. Then it clears its MIE flag. Now, as you know, we know that this MIE flag, if you just write a 0 into it, that actually means interrupts are disabled.

So this action, this first action of the CPU, is actually quite clear. It is telling us what? That is to say, when we go and execute the second half of its work, our CPU is actually in a state where interrupts have been disabled.

Because at this point, even if an interrupt occurs, it won't come and interrupt us. This concept is very important. So what this step achieves is that interrupts are disabled. That's the first action. Now, what does the CPU do as the second action?

It will set MEPC, and at the same time, PC will be set to this value here. Now, this setting of MEPC, is actually also for use when we return later. Because we know that this MEPC is essentially where, when we return, when we execute mret,

it will read out the corresponding value from here, and then jump back. Now, as for the setting, So, as for the specific setting, the specific setting, does it set the PC value or the value of the next instruction? Well, it will depend on this:

whether what we currently have is an exception or an interrupt, and handle them differently. We've written this out for you as well, meaning that if it is an exception, then the MPC points to the instruction that caused the exception,

whereas if it is an interrupt, it points to the instruction after the interrupt. What does this mean? Well, we've drawn the two figures below for you, so let's work through them. The meaning expressed by the figure on the left

is that when an exception occurs, for example, while it is executing this instruction, this instruction instruction2, say this instruction is an exception instruction, You made a mistake when you compiled it You wrote an exception instruction

An instruction we don't recognize got written in there But if it's assembled at compile time, it will definitely tell you Suppose we hard-code it, and we just write an exception instruction, or say we write something that accesses something it shouldn't access,

all of these are possible, triggering an exception. Now, when this instruction is executed, assume this PC, the PC is currently pointing here. So the PC is likely the address of this instruction. Now, when an exception is triggered,

it will save the value of this PC into mepc. And once it's saved there, as you know, when it triggers an exception, it goes off to execute an exception handler. After the exception is handled, it will come back. Now, when it comes back,

it actually returns based on this, based on the MEPC you saved. So, since when you jumped away, what you saved was the value of this instruction, then think about it — when it comes back, it actually returns to this same instruction,

and starts executing this instruction again. So that is actually the effect an exception has. Now, there is definitely a reason for doing it this way. Because if you think about it, for example, we... what is it actually trying to achieve here?

That is to say, right when this exception occurs, it gives us an opportunity — from the programmer's, from the CPU's perspective — You can actually understand it this way: that is to say, when it executes this instruction, it decides that this instruction

has triggered some abnormal behavior, and so it goes and gives us a chance. This chance is the programmer's. When the programmer gets this chance, they go and write an exception handler function. This, it expects you to, within this

exception handler function, take this exception, this problem that was triggered, and solve it. After it's solved, then it comes back, So it's like saying, it gives you one more chance to retry and execute it once again. Then it's possible that

when handling this exception, it will actually fix the problem that caused the exception in the first place. So, this is what we call an exception. So an exception, what it actually interrupts is the current instruction. After it's done,

it will come back, and re-execute the instruction that triggered the exception. If we go a bit deeper, for example, you might If you've also learned about for example, a page fault exception, which gets triggered all the time in Linux

this page fault exception, actually, think about it, a page fault means you find that the current instruction is not in your memory, then it might this is the exception being triggered, and it will make you do what? Go fetch the page,

swap the page in. After the page has been swapped in, this instruction is simply re if it gets re-executed, doesn't that solve the problem? Think about it, isn't that the case? So this kind of thing is an exception. An interrupt, on the other hand,

That is not the idea here. Because with an interrupt, the actual intent is, what it wants to achieve is this kind of effect: that is, say it is executing this instruction, and an interrupt is triggered. Now, an interrupt, in fact, is generated by an external peripheral.

So it has to go and run a piece of logic for that peripheral, maybe to read a piece of data from an external device, or something like that. So, when it is handling the interrupt, and it saves this MEPC, what it actually saves is the address of the next instruction.

So this way, after it finishes this one, of course it will finish executing this instruction first, and after it finishes, It goes off to execute an interrupt handler and once that's done, once the handling is finished, it comes back.

And when it comes back, it actually returns to the next instruction. So in this way, the effect it achieves is that, from the point of view of our normal execution flow, it's basically as if nothing ever happened in between. Because it's actually an interrupt,

not an exception. Does everyone get this? So this interrupt actually went and did something else, but while it was doing that, it didn't want us — from the programmer's point of view, that is, you should feel like this whole thing

as if nothing ever happened So it actually when it comes back, it executes directly executes the next instruction at this point So this setup of MEPC could be a different value Later, when we talk about interrupts, I'll explain to you again

this related concept just mentioning this to you here So this automatic step the second step is setting MEPC, and at the same time, PC is set to this Now, knowing this, once you know that PC is set to this, it actually achieves this jump

it actually performs the jump jumping to the exception handler to execute And then this The next step, it sets mcause according to the type of this trap. There's nothing much to say about this. It just tells us this: the specific exception or interrupt that occurred,

exactly what kind of exception it is, or what kind of interrupt it is. Then later, when we go to the bottom half, we'll know how to handle it. And the last one, it saves the privilege mode from before the trap occurred in its MPP field.

This is also used for restoring it later. Then, it changes the privilege mode to Machine mode. What this actually means is that no matter which privilege level a trap is triggered at, the hart first switches to M-mode to handle it.

Now, some of you might say, doesn't it sometimes also enter What about supervisor mode? This one, as you can see, it is actually implemented through a so-called delegation mechanism. This is not something we use in our course, so I will just mention it briefly here.

But at the very least, we need to know that no matter what mode you are in at the moment, once a trap occurs, it will first definitely enter machine mode. It enters machine mode first. You all need to keep this in mind. Alright.

This is what we call the first part of the trap handling process. As for this part of the process, it is all done automatically by the hardware. Then, once the final So what's the final, final result? Well, it will jump — it starts jumping to our

trap entry function, to execute our program. So then we enter this second half. The second half, as we said, is about executing a piece of our program. Next, let's take a look at the third part of trap, which is the bottom half.

The bottom half, as the name implies, means that this part needs to be written by us software programmers, a trap handler function that we write ourselves. So today, We're mainly talking about exceptions, so we actually have a piece of

exception handler code. Now, we say that whether it's an interrupt or an exception, as a unified trap, what should it actually do? Well, we can actually take the entire process and summarize it roughly into the following five steps.

So the first step, we say, is to save the control flow context information. Now this concept of context In the last class, when we were talking about context switching, we already covered this, right? That context, in fact, is just

when this exception occurs, on its processor, what's in the hardware -- you can think of it as the values of those general-purpose registers. So in fact, x0, we don't need to save, right? So x1 through x31 -- the values of these registers,

we save them. That's what we call saving the context information. As for mscratch, we also mentioned it earlier. That is to say, we use this this register, we actually use it to save this. This is actually also in mode mode, this Trap-related register.

We use this register to save this, the current memory pointing to the current context, so we use it to save its information. And correspondingly, on our side here, correspondingly, our right side is actually some sample code. You can take a look at this code yourselves.

And actually it's reflected right here, it's this piece of logic. Now, this piece of logic, what we actually do at its core is call a REG underline Save This is a macro for register save This macro is defined in the assembly like this

Now, this macro we're writing here we introduced to you earlier Now, this is actually a parameter In other words, the macro takes a parameter You can understand this parameter as it is actually the value swapped out of mscratch

So what it actually is is a pointer to the base address of a piece of memory the base address of the memory and inside this piece of memory is where the context is stored Now, within the context starting from x1 all the way to x31

these registers are placed in this context So we just take from the CPU these data and by saving, what we mean is taking the values on the CPU, the values in the CPU registers, and saving them into here so that's the save And naturally, corresponding to the save,

what that means is that once the processing is done, there has to be a restore, right? So let me mention this in advance, the restore is done right here, also using a corresponding macro to carry it out That is the restore Now, between these two,

what we say is the main core here is that It's going to call our handler function. Now, because the handler is generally already fairly complex, this part is usually written in the C language. So we actually call a C-language function to implement it.

Now, this calling process is actually what we covered earlier when we talked about assembly, which I introduced to you back then. That is, how to call a C-language function from assembly, right? Now, a0, a1, you still remember these, right?

They must be what? What are they used for? They're actually for passing parameters. This trap handler, we'll see in a moment, actually takes two parameters. One is used for We actually just take the values of mepc and mcause, and pass them in.

Now, as for mcause, we know it is actually used to hold the specific type of the exception or interrupt. Once we pass it in, inside that function there will definitely be a switch-case. As for mepc, when we pass it in, later on I will show you

that actually inside this function, we may modify this value. And the return value of this function, is actually mepc, so after it returns, We will take this return value We return it through a0. This is all stuff I've introduced to you before.

These are the function calling conventions of RISC-V, the function calling convention, right? If you're not familiar with it, go back and take another look. In other words, why do we put it in a0? Why do we put it in a1? All of this was covered earlier.

So what's actually going on here is this part corresponds to calling a trap function and handling its return value. Then the final step is returning by executing the mret instruction. What exactly mret means, we'll cover that later.

In short, our entire trap handler, no matter how it's written So the overall framework is really just these few steps. The change, the change is in this trap handler function. As for our function, we'll take a look at it later. This is the second half,

what our software needs to do. It's really about writing a handling function, this vector, and then taking this vector, which is a vector, and taking this function, putting it up front, during initialization, saving it into that mtvec

register. Of course, right now we're using that direct mode, not the vector mode. Because ours is relatively simple. That's the third step. So the final step is returning. Returning is actually reflected in the code we just saw,

the call of that mret instruction. So this instruction, it's very important for us, and we should understand it. That is to say, this instruction, it belongs to the privileged instructions. This privileged instruction, it's specifically used to help us return from this,

this exception, or rather, return from an interrupt trap. So this instruction, there are several kinds. As we can also see here, there's mret and sret. There is 'rewritten' So what it really means is it corresponds to which privilege

mode you are returning from If you are returning from machine mode, you use this one. If you are returning from supervisor mode, you use this one. That's what it means. But in what we are doing here, all the demos I show you use mret,

so for different privilege levels, how they exit, each has its own return instruction. Please keep that in mind. Now, if it is, what we are talking about now is mret, then when mret executes, what happens? This is what we need to focus on understanding.

We simply called an M return. So what does the CPU do? It does a few things. The first thing, it will take the privilege level that was held, as you remember, before we entered the handler function, the CPU would save its privilege level into this field.

So now, putting it back at this point, is actually restoring it. It restores the original mode. Because whenever an exception or an interrupt occurs, it always wants that, after exiting the trap, the original state is restored.

So it also restores the privilege level. And then, after this value has been used up, it will set it to a value, setting it to user mode. If user mode is not supported, it will just fall back and write it as M-mode, and that's fine.

So that's the first thing. The second one is, the first thing is to restore the privilege level. The second thing, actually, is to restore the interrupt status. It saves this in mpp, and restores this value back into mpp. Now, as you know,

if from the very beginning, under normal circumstances, we generally have interrupts enabled, and then an interrupt occurs. Once an interrupt occurs, as we discussed earlier, it will disable the interrupts. So this is what this sentence means:

when it returns, Actually, it's just it was originally enabling interrupts so it's basically saying the interrupt gets enabled again So this instruction in most cases achieves the effect of enabling interrupts Of course, you have to look at

before entering the interrupt before entering this trap the original state of the interrupt it is restored based on the original value And after the restore is done it will also set the MPIE back to 1 If you're interested, you can debug it yourself

and take a look while stepping through at the values in these registers and compare them with the spec and what we covered in class to cross-check and deepen your understanding of it This is the second step So after restoring the privilege level and the interrupt, right,

the last step, it takes mepc, which we know was also saved before entering the interrupt, right? So it takes that saved value and restores it back into the PC. You can imagine what effect that has, right? It jumps back. It returns to the state before the interrupt occurred.

Now for an exception, as we mentioned earlier, it returns to the position of the instruction that triggered the exception. So that instruction which triggered the exception will be executed again. That is where exceptions differ from interrupts.

What we're covering today, mainly starting with exception handling, this is our trap, The fourth step of this handling, the last step, is calling mret. That is what happens. So once the whole process is done, we say that this entire trap handling process

is actually complete. So that is the whole process —it's just this one process. Take this diagram, when we go back ourselves combine it with what we covered in class, and go through it yourself. Now next, we will take the corresponding code

and walk through it briefly. As for the related initialization code, we have already covered it. Today we're covering this one. The fifth one, 05-traps. It's this example. This example. corresponds to what we're covering in today's lecture.

In here, 01, 2, 3, 4, don't map exactly to our chapters. This side matches our steps. As for the chapters, for today's material, it should be Chapter 10. Now, for traps, we won't go into the initialization part here. For this code,

the main difference from the previous step, is that we've introduced a trap.c. This contains the trap-related logic, including initialization and so on. Initialization is very simple. So the main thing here is the trap handler.

This is the C code inside the trap handler function. As we know, in the middle, it will jump to this function to execute. Let's take a look. What does it actually do in here? The example we're giving you now is very simple. It actually takes two parameters.

One is the EPC passed in, and the other is CAUSE, right? So what this really is, well, the most core part, is that it makes a judgment based on this CAUSE. Now, as we mentioned earlier, this CAUSE, which is MCAUSE, with MCAUSE, the highest bit,

if it is 1, it means it came in as a that is, what happened this time is an interrupt. If it is 0, it means it's an exception. So it will take this CAUSE and 0X8000 We do a bitwise operation on this value So this process actually figures out

If it is true Then what just came in is actually an interrupt Otherwise, it is an exception Otherwise, it is an exception So I just wrote here That is, this part of the logic is actually interrupt handling Now, here In this lecture, we'll leave it aside for now

We'll focus below on exceptions So if it is an exception We call it Generally, we have a term for it called a synchronous trap Because we know an exception is actually triggered when you execute this instruction Right An interrupt, on the other hand, can be asynchronous

It can happen at any time So this exception We also call it a synchronous trap, and that is exactly the reason. Now, for this part, as we mentioned earlier, if on this side, I actually added a panic here. The panic here, you can take a look at the code,

is actually an infinite loop. Let me give you a quick demonstration. That is to say, once an exception is triggered, it will come into this logic. The reason I added an infinite loop here, is actually to make sure that it won't be executed repeatedly,

because as we mentioned earlier, once an exception occurs and is handled, when it returns, it will go back to the very instruction that triggered the exception. We can first take this panic that is, remove this infinite loop,

and see what happens. We'll just get a feel for it. So this, this test program, is this function. This function, it's trap_test. This function is actually in user.c. It will be removed here. That is, we're in this example, since it's based on the previous

cooperative multitasking example, modified from it, so we're actually inside the user_task0 task, after we come in, as soon as we come in, we do nothing at all, and just call trap_test directly. and then it will trigger this exception of ours

As for triggering an exception, there are many ways to do it. One way is, what is this line doing? Take a look, everyone. This line is essentially saying, I write a value to the address 0x00, right? But as you all know, this address is actually

not within our legitimate memory map table, the Memory Map table, it's actually an illegal address. In fact, it will trigger an access exception, an access failure. So, once this exception occurs, as we know, once the program As soon as this instruction executes

based on our understanding, this exception should get triggered. Once it is triggered, it should go into our trap handle function. Inside the trap function, we will see it go and print this message, print it out. Of course, after we print it out,

since we didn't do anything here, it will return. After it returns, as you know, when it returns, as we just explained, it will return to this instruction of ours, and continue with the instruction. So, if we were to... Without this special handling,

we would see this print statement being printed over and over again. If you see this happening, think it through based on our explanation: this is an exception. After the jump returns, it comes back to this instruction, and continues to re-execute it.

Let's see if that's really the case. Let's recompile it. Let's run it. It just keeps printing like crazy. It almost won't stop. I had to force it to stop. As you can see, it just keeps printing this one line over and over again.

Right? So why does it print this one line? I just explained it to you. That's exactly the reason. Now, of course, in the code we gave you, we actually added a panic. With the panic added, it's actually an infinite loop inside. It just stops printing.

So if we restore it to this state, let's run it again. You can see it actually stops right here. And stopping here, well, it basically prevents the exception from continuing. Actually, the exception never returns at all. So through this example,

we got a sense of how exceptions work. Now, of course, suppose you still want to come back, there's actually another way. That is, you force a change to the return address yourself. Because, for example, on your side, didn't we

already pass this return value back through the sret? So what we can do here is forcibly add 4 to this value. In effect, what that means is it won't jump back to when it returns, it won't go back to that original instruction, but instead to the instruction right after it.

And as for the instruction right after it, that's essentially equivalent to this being commented out. So it will actually go and print a "Yeah I'm written back". Let's see the effect. We remove this infinite loop, and then we forcibly add 4 to the return value.

Adding 4 actually means you know, because on 32-bit, the way we're compiling it now, one instruction takes up 4 bytes. Adding 4 actually jumps to the next instruction. The next instruction. Then, as we discussed earlier, that is, for this Entries,

when it returns, we know that there is a it will, based on this, based on the returned a0, rewrite the return value to overwrite mepc. We know that overwriting mepc, in effect, will cause that when it does mret, it returns based on this value.

So once we take its return value, and add 4 to it, it will actually jump to the next instruction. Let's take a look at this, and see if that's the case. This is the modified version. Let's run it. As you can see, Now you can see here

the exception did indeed occur But after this exception happened, it did not enter an infinite loop. It does not repeatedly execute this instruction again, but instead prints out this line: I'm returned from trap So at least it can come back,

as I just introduced to you. That is to say, it is precisely because of this instruction, once the exception is triggered, when it returns, when it jumps back, it actually comes back to here, comes back here, and so it returns. Now over here,

what comes next, is this task: and it just keeps running. Of course, here, because of this trap test, it should be executed repeatedly here. The main point is that it will continue executing on this side, so this exception will be inside this while loop here,

and will be triggered repeatedly. But anyway, let me demonstrate this for everyone here. That is, we can actually modify its return value, so that it doesn't necessarily mean skipping this, so that it won't repeatedly execute the exception handler,

it will not re-execute the instruction that triggered the exception. Through this example, you can go back and take a look at it yourselves. Now, regarding this trap and the concept behind it, as well as what's involved with exceptions,

and this kind of basic handling, we'll wrap up our introduction here.
