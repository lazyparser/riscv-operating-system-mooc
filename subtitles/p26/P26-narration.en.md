# English Narration Script

Alright, let's move into Chapter 16 of our course. This chapter is also the last chapter of this course for us. The topic of this chapter is called system calls. In fact, by working on this simple system, we'll introduce user mode and kernel mode — the kind of things you only find in real operating systems. We'll also see how, based on this kind of security protection, we can implement some

the concept of system calls. The content of this chapter is mainly divided into these parts, five parts in total. First, I'll introduce to you the concept of system modes — user mode and kernel mode. Then, we'll take a look at how, with our system modes,

we can switch between these system modes, right? And after that, once we have this concept in place, we can go ahead and implement our own system calls. Now, the system call What does this execution flow look like? And how does it pass parameters

between user mode and kernel mode? And finally, I'll introduce to you some concepts around how our system calls are wrapped. So, as for the reference documentation, we mainly rely on the basic specifications of our instruction set.

Let's start with the first part, which is the concept of user mode and kernel mode. Now, on this topic, when we introduced the RISC-V instruction set specification earlier, we actually already mentioned this concept. We also mentioned that

this RISC-V instruction set can support our system being implemented in these three typical application scenarios, numbered one, two, and three. The first one, we said, is the simplest embedded system. So in that case, it only needs to support one mode,

namely machine mode. Now, going a step further, we can say we can support a so-called secure system with a certain degree of security, an embedded system like that. So, if we want to implement this second kind, that is, an embedded system with a certain degree of security,

then, it must support at least two modes, namely machine mode and user mode. So, up to this point, the system we have actually implemented, is really just the first kind, because in our current example, all the code, our mode Actually, it has been running in machine mode all along.

We never entered user mode. Right? Now, the third one is this: Of course, we haven't introduced the supervisor mode either. In other words, the concept of virtual memory, actually virtual addresses, none of these concepts have been introduced.

So, up to this point, we've only implemented the first kind. Now, to implement this kind of so-called secure embedded system, we naturally need to be able to support two different modes. Taking this diagram as an example, it effectively means that

our program must be able to implement the normal user-mode user programs We run in user mode. Then, whenever we need to execute some kernel logic, we switch the mode down to the red part, that is, into MachineTag to handle it. And then,

once the handling is done, we switch back to user mode. If we can implement this approach, it's essentially equivalent to the second type he mentioned, namely the secure embedded system, in this kind of state. Now, in this situation,

we can actually do the same thing: we can call this UserTag user mode, and our M-mode is our kernel mode. Like what we usually have in Linux, When we say user mode, we mean that the kernel mode is what actually executes it and kernel mode mainly refers to

the supervisor mode. So here, we can actually just talk about the kernel mode as the machine mode. That is completely fine. So how do we make our mode transition between two modes instead of staying in just one? So let us review

the examples we have done so far and how they were implemented. That is to say, how did we make sure that all of our code stayed in the machine mode? So for this, we need to look back at our earlier implementation. Now, let me pull out this piece of code for you.

That is, in the earlier part, we may not have emphasized this enough, which is to say, you can go back and review how we, in... because from Chapter 8 onwards, after this preemptive mode was introduced, in fact, we basically...

this task scheduling, this user task, in fact, was finally settled in its current form starting really from Chapter 8, 08, this preemptive... starting from this point. Now, let's pay attention to this: that is, at that time we,

in order to ensure that our... all of this, the user-mode tasks, as well as the kernel mode... that is, our user... User tasks, the code of user tasks, and the code in our kernel all run in Machine mode. Actually, we have a piece of logic

that I don't know if any of you have studied carefully. So let me bring it up for you again. That is, in start.s, this file, this assembly file, which is our entry point, inside this entry assembly code, before we jump into the start kernel,

we actually have a piece of logic. So let's recall first, that is, back then, what did we do inside this start kernel? This start kernel, what did it do back then? So in start_kernel, following the program, if we trace this code,

then we get to start_kernel. First, after entering start_kernel, we call a function, schedule. And before that, there's all this dot dot dot stuff, which is really just a lot of initialization logic. We won't go into that. So at the very beginning,

actually in this state, up to this point, we're still all in machine mode. Because right after boot, it stays in machine mode. So then we, finally, call the schedule function. Now, this schedule function, actually internally, it will start

all of our user tasks are actually started up inside this So after we say they are started, in fact, this the one where start kernel resides, this one we mentioned, this is also a control flow, and this control flow will actually no longer run.

So the things below here shouldn't be printed out, which means the logic below is actually never reached at all. So how do we go about starting the very first user task inside schedule? We do it by calling switch2, right? You all still remember this,

this switch2 So with this logic, what we ended up doing was running through a bit of assembly logic, switch2, and in the end, we actually used the mret logic, to enter our first task at its first instruction, and start running from there.

So now let's go back to what we just... back to what we just... the earlier part, this start function. What did we actually do in here? Take a look at what we did in here. Right here, we can actually see that what it's really doing,

let me briefly walk you through this piece of assembly. So what it actually does is, first, it loads a value into the register t0, assigns a value to it. And this value, as we said, is actually produced by, taking this 3, which we know is actually 11 in binary,

left-shifted by 11 bits and 1 left-shifted by 17 bits, and combining them to generate a value, which is then put into t0. And then, as for the CSR instruction, it performs a CSR operation. We know what that does, it reads this CSR register.

It's really just taking mstatus, the value of this control CSR register, and reading it into a1, and then what do we do? We do an OR operation, taking the value in a1 and the value in t0, so these two values, we do an OR, we do a bit-set operation,

and after doing the OR operation, finally we take the result of the OR, which is this t0, and write it back into mstatus. This is the write operation. This is the read operation. So the effect this actually achieves, is that we are essentially modifying mstatus,

modifying this register. Set some of its bits to turn them on That's basically what it does Now, what do these bits do? We can refer to our manual regarding mstatus the description of this register This 3 shifted left by 11 bits Actually, as you know

corresponds to the MPP bits This 1 shifted left by 7 bits actually corresponds to the MPIE bit Now, as you know what is the effect of setting these bits? These MPP bits let's recall what they mean They represent, in machine mode,

the previous privilege level So once its value is saved, What effect will that have? So let's go back to what we just said about what happens with the mret instruction This is a review Once this mret instruction is executed, based on what we introduced earlier,

we know that it takes the value in MPP to set the privilege level after mret, this 'hot' privilege level, so if we set it to 3, we're actually setting both of those bits to 1. And after that, we're back at the Machine level. So what that means is,

after this mret, we know we return to, we reach and enter into, the instruction sequence of the first task. And at that point, the first task will definitely be That is to say, it's basically like saying keeping the first task still running

under Machine mode, and that's actually what it means. And then, this MPIE field, is actually the interrupt enable bit from the previous interrupt. So if we set it to 1, it effectively means that we are turning on the interrupt enable from before.

Now, we know that at the time of MRET, it will likewise, based on the value in this field, restore the value of MIE. And we know MIE is used to control whether interrupts are enabled or disabled. So, that means this instruction,

the setting here, effectively ensures that when it enters the user program, for that task it enabled interrupts again enabled them again So that's essentially what we did Of course, what we care about is actually over here we're not particularly emphasizing

enabling and disabling interrupts here What we mainly want to stress is this that is, this thing we're basically, so to speak, ensuring that when we enter the user's task, after the first task, after mret, our hart, that is, the CPU's privilege level,

still remains in Machine mode This is what we have so far So our current system is one where, whether it's a user program or kernel code, is actually running in Machine mode. That is how it is implemented. So now we are going to

make a change, and the idea is this: after entering the user program, we want to move down from Machine mode and return to user mode, switch to user mode, instead of staying in Machine mode. So how do we do it? Actually, it is quite simple.

As we know, this register mstatus, this register, after power-on, defaults to all zeros. That is to say, as you know, the value of MPP They start out as all zeros. If we don't change this value, then after calling mret, the new instruction,

when it runs, will read the value in here to set the CPU's mode value. So if it's 0, then 0 actually means user mode. So if we want to enter the user program and switch to user mode, what we actually need to do is simply not to set it to 11.

Make sure that after the mret, it will naturally enter user mode. Keep this in mind, everyone. So in our new code, this is the old code, and after we move into the new code, which is in our 11-syscall, Now, to make our user task

not stay in Machine mode, but instead switch to User mode, it's actually very simple — just don't set the MPP bit. So you can see that in here, at this point in the new code, we only turn on interrupts, because interrupts still need to be enabled.

Think about it, right? When we run user tasks, interrupts still have to be enabled, because if they're not enabled, interrupts can't come in, and that obviously won't work, right? So here, you just don't set this bit, and that's it.

So after removing this, In the new code, why can we enter this kernel mode, why can we enter user mode? Well, it's actually done through this. Let me explain that to you. Now let me show you. Once we have this user protection mode,

we need to understand that once a user program is running in user mode, then if it tries to access resources that can only be accessed in kernel mode, it will actually be restricted. Let's experience what this restriction looks like.

We can open our Syscode code. We have a user.c here, this test program. Now, this test program, as we can see here, There is a piece of code like this. That is, this this macro here, what we mean is, inside this, we, if this macro is enabled,

if it is defined, it effectively means that, we have added support for this system call. So when we just looked at this sysstart.s, let's take a look at start.s, right at this spot. If we have enabled this system call, we will not set that MPP flag bit.

Do you all see that? Otherwise, we would still go ahead and set it. And if we set it, that means it still won't switch down into user mode. So now, let me show you something. What I mean is, right now we want it to switch down into user mode.

Let's take a look at how, after switching down into user mode, there are things that can only be accessed in that mode, things that only the kernel side can access, and we can protect them. How do we protect them? We can assume that right now we

just want to read, to open this up, and after you go back, You can also follow along and practice with me. That is to say, suppose... As we know, in the past, our code would actually call this everywhere, and that was no problem.

Why? Because this logic, it actually reads mhartid, a register in M-mode. Now, this register, it can only be accessed in certain modes. In user mode, access is not allowed. But before, it didn't matter. Because back then, we never switched to user mode.

So calling this function everywhere, there were actually no restrictions at all. None whatsoever. Because we were always, in fact, at the highest privilege level. No restrictions at all. Now, as we know, once we turn on that config syscall,

our user task, starting from this instruction, has actually already switched over, already switched into user mode. So in user mode, if you go and access this, go and access this counter, then that is definitely not allowed. So it will trigger an exception,

trigger an exception. Let's take a look, and see if that is what happens. So let's turn this on. We don't worry about the rest below, because once an exception is triggered, inside the exception handler we will actually enter panic,

and it will actually just hang. So the program just can't keep running. As long as we see this exception triggered, that proves what we said — that in user mode access really is restricted. Let's run it. Everyone, look here — you see this spot?

Right after Task0 is created, do we know why a synchronous exception is reported here? It's actually because we are, in the current mode, actually already in User mode. So if in User mode you go and access that, if in User mode you go and access this instruction,

access this register, then it will definitely raise an exception. So this next line down will actually never be printed. Why? Because once you go in there to access it, it actually goes straight into the exception handling logic.

And then it will oops. So this actually serves as a to user mode, some of our kernel-mode resources are protected. This is what we call security, which is reflected right here. Now, how do we implement this user mode and kernel mode,

that is, how do we switch out of user mode? Actually, let me explain this to you here. It's this statement. Pay attention to this. Now, next we need to there's a question, which is, suppose we are in user mode, suppose we are in user mode,

and we want to get the hart ID, then what do we do? Because we can't directly call Calling it directly would cause an exception. So what do we do? Well, this involves having to that is, we have to further wrap this thing up. Now, what do we mean by wrapping it up?

It really means providing system calls. And providing system calls means that our kernel takes this access, this actual operation, and wraps it inside a system call. Then, when a program in user mode wants to access this thing,

what we actually do internally is take this mode and switch it, actively switch it into Machine mode, and only then can we access this HotID. Now, this process of actively switching It is essentially a way of actively triggering an exception

and this kind of exception is what implements the switch between our system modes. And as for this mode switching, we say it happens at these two points: when switching from user mode to kernel mode, we actually use a special

instruction called the ecall instruction to do it. We will introduce this ecall instruction to you later. In essence, it simply triggers an exception. And once the exception is triggered, it enters the exception handling routine.

And upon entering the exception handling routine, it actually switches to machine mode to handle this exception. Then, in machine mode, it naturally can — it is equivalent to entering kernel mode. Kernel mode. So in here, we can access those privileged instructions,

and access those privileged registers. Then, once that's done, since we need to return to user mode, we need to make the return. When returning, essentially we are making use of the e-return that we learned about earlier, to implement it.

This one, we've talked about this instruction before, so I won't go into it again here. Let's focus on this e-call instruction. The e-call instruction, it is defined in the privilege specification, right. So it is called the so-called E-Call.

That is, Environment which means the environment call the environment call is actually what we specifically refer to as it being that full pipeline switch so it actually belongs to a kind of actively triggered exception an actively triggered exception

so after this actively triggered exception occurs, depending on which call exception you trigger at this point when calling this E-Core, when you currently call this E-Core, the current value of the privilege level we distinguish different exception codes.

You see, everyone, we have 8, 9, 11 here. So 8 actually means If you make an environment call in user mode, then the exception code triggered is 8. If it's triggered in S mode, it's 9. And if if the environment call is made in some other modes,

then the exception code triggered is 11. So now, since this exception is triggered in user mode, it should be, as you can probably guess, it corresponds to 8. Right? You'll see it later in the code, it's 8. So that's the second point.

Now the third thing to note is that, this is, after all, still the basic exception handling we talked about earlier. So when an exception occurs, we know the value in the EPC register, it holds a return address. Now, one thing to pay attention to

if the exception is triggered by the 'yi' instruction (the illegal instruction exception), then what the EPC register holds is the address of the 'yi' instruction itself, that is, the address of that very instruction. So I want to remind you here,

and this is worth paying attention to, that if you want execution to continue after the exception is handled, then we actually need to modify the value of the EPC register inside our exception handling logic. We need to change it

so it points to the next instruction to execute. Otherwise, it will return to the current EPC, and you will end up in an infinite loop. Keep that in mind. Keep that in mind. This is related to ECO, an introduction to this command.

So next, let's take a look at the execution flow of this system call. Now, this diagram here is a fairly classic system call flow. In fact, for any operating system, the invocation process is pretty much the same. It's just that I tweaked it a bit

to fit our RISC-V, into our current example here. So, for instance, right now, on the user side, as we know, the user programs we write effectively start from the left here. So we go call a function, say this function. Of course, this function is written fairly generally.

For example, what we're learning now, those system calls in Linux, say open, read, write, they're all this kind of scenario. After you call this open, after the function, it will enter this open function. So inside this open function,

it does some logic. Of course, the most core part, what we care about is that internally it will definitely call an ecall, because only by calling ecall can it trigger this exception. Then this exception can eventually reach switching this state from user mode

to kernel mode. Now after switching to kernel mode we then enter our unified — it still goes into our this trap handler function Now inside this trap handler function, we can go ahead and do our that is, at this point for example, if we need to access some kernel

logic that can only be accessed in kernel mode, then we implement it inside these functions Now after the implementation is done, we know that in the end we still have to return And when returning, essentially it's still calling mret

there's nothing much to say about that because for this exception, in the end it always calls mret After mret, then we come back and after coming back, the whole process is complete. In other words, the entire system call execution flow

actually works the same way. Now, at the same time, let me also mention here that after this mret, the mode switches from kernel mode back to user mode. Because, as I mentioned earlier, in the mret logic, we actually make sure that

since with mstatus.MPP, as long as the first time we enter is in user mode, this mstatus.MPP will preserve the previous logic. The previous logic is user mode, so when it returns, it is still user mode. It returns to user mode. After that, there's no need to

We don't actively set it again, because as long as when it comes in at the first time, we set MPP to make sure it is zero, then later on it will naturally be saved and restored again. I won't go into more detail on this. This actually belongs to

the unified handling logic part of this exception. If it's not clear, you can go back and look at our lecture on trap and exception and the relevant content in that chapter. Basically, this is it. Equo, on the other hand, will cause the mode to switch to kernel mode, and mReturn will switch the mode back.

The mode will switch back. The mode will switch back. The mode will switch back. The mode will switch back. The mode will switch back. The mode will switch back. The mode will switch back. The mode will switch back. The mode will switch back.

The mode will switch back. So this is one of the system calls, the overall execution flow. Now, more specifically, how does our code actually get executed? Let's take a look at it together. So the logic in our code is designed like this.

First, let me walk you through the overall process of this code. We've added a few files, for example, an assembly file called ucs.s. What's in there is actually the definitions of our functions, which you can basically think of as

these functions essentially corresponding to this part, this part here. When you look at this code, it all looks quite similar, doesn't it? called in one go So these functions, even though they are written in assembly, are actually equivalent to this kind of

this logic, this part of it. Now this part implements a call that runs everything in one go. And this one-shot call, as we know, once it is made, what it goes to is what we have seen again and again before, which is the trap vector inside N to S,

that is, our trap entry point. Now this entry point, the process it goes through, is not a simple function call. We use blue to indicate it. What it actually represents is triggering an exception. It actually represents triggering an exception.

And inside this exception handler The core thing is that it calls the trap handler. So the trap handler... If we trace into this function, we'll find that it actually ends up here. As for why it's 8, we explained that earlier.

It's because we triggered the exception in user mode. We triggered the exception. So this trigger of ecall... so the code value is 8. So inside 8, inside the corresponding entry 8, we actually call the kernel's syscall handler function.

And then, inside there, based on some specific parameters, we call the actual SysGetHID. Inside this, it will access the earlier... For example, just now we... Taking the example we just used... Then here we will go and actually access that HotID of his

So in this situation, since the access is done in Machine mode, naturally there won't be any exception. Now, among these things, they are actually all ordinary function calls. After returning, please pay attention here. There is a key piece of logic here,

which is that at this point we need to add 4 to returnPC. Why? The reason is what we talked about earlier, which is this equal function. After it triggers an exception and returns, if we don't handle it specially, its EPC actually saves

the equal instruction itself. So if we don't add 4, Now, after the exception returns, it will continue executing this equal instruction, and then this loop starts happening all over again. Now that's not what we want to see, because what we want is to move on to the next step.

So we forcibly take this MPC return PC and add 4 to it. We know, based on our code logic, after this return PC returns, it will go and modify the EPC register. Now, why add 4? That's because one instruction is four bytes. Right.

So after returning this way, we know that this one, it will not, it will not, it will no longer jump over here to execute. So on our side, it will be able to continue moving forward. Right over here at this point it hasn't exited yet

so this return, after mret returns, it can continue moving forward at this point, it proceeds down to here otherwise it would fall into an infinite loop at this equal branch and that's not what we want to see so this mret, when it comes back,

is also shown with a blue line because this is not a ordinary function call either because it involves a mode switch because when it comes back from here, the mode again goes from supervisor mode back to user mode so please pay attention to this

This is an actual code execution process that corresponds to the flow we described above Take a moment to get familiar with this So once we have an overall understanding of this execution flow, there is still one problem we need to solve

and that is the issue of passing arguments in this function call Because this process is not an ordinary function call So during this process, similarly, because we for example, when we call the open function system call, we may still need to, at the user level,

pass some values into the kernel level So how should we design this passing process? This is the fourth part we want to introduce to you Now for this system call, As we know, it is actually the operating system's set of interfaces provided to the outside world

So the system call's own parameter-passing convention, is in fact handled by different operating systems themselves through implementation-defined mechanisms. However, to a certain extent, it must also follow what we mentioned earlier,

the function calling convention. The function calling convention is fundamental. However, the operating system can, on top of that, on top of this function, on top of the function calling convention, on that foundation, make further specifications

and further rules. So the little system we've implemented so far, let's call it RBOS for example, our current definition, is actually modeled after the classic Linux system calls. So we have a few rules, some rules. The general principles,

are listed in the three points below. First, there's a concept involved, which is the concept of the system call number. The system call number. So what does the system call number mean? On this, let's come back and take a look.

That is, let's come back and take a look at the ecall instruction we mentioned earlier. One thing to note about this instruction, is that inside it, it actually doesn't carry many parameters. without too many parameters So this one,

but, when we talk about system calls, there's definitely not just one, right? With ecall, it just triggers the system call, but our actual system calls could be numerous, right? For example, look at Linux — there are many system call functions,

open, read, write, lots of system calls. So how do we tell these system calls apart? We actually do it through what's called a for each system call, we give it an integer value, a number, to distinguish them. But, Now, this number,

there's nowhere in the ecall instruction to put it. We don't have a place to store that value. So under the current convention we're using, we actually make use of a register. This may differ from some other architectures. It might not be quite the same.

In some architectures, there might be, in this, there are also some related trap instructions, and within the trap instruction itself, the system call number is included. However, with RISC-V, it doesn't do it that way. It actually,

by convention, puts the system call number, on a parameter register called a7, one of these argument registers. As we know, the ones starting with a is used to pass arguments. So this special argument is used to hold the system call number, as specified.

Besides that, our a0 through a5, they hold the other arguments of the call. Actually, the system call number can also be seen as one of its arguments, but this argument is a bit special. We can use this argument to tell different system calls apart.

As for the other arguments, a0 through a5, they hold the regular arguments. Regular arguments. So at most, that's equivalent to 6 of them. Any more than that, and mainly, considering efficiency, in principle, you could hold more.

We could also use A6. We could put one more there too, right? But we don't use it. Then there's also a return value. As for the return value, it's placed in A0. Generally, for system calls, the most basic return value is just success or failure.

As for the other parameters, they may be returned through some other means. But the value returned in A0, mainly indicates success or failure. Success is generally 0, and if it's an error, then it's a negative number, representing different errors.

This is our convention. Actually, Linux does the same thing. We made this rule by referencing Linux. So based on this principle, we can carefully design how our system call should be implemented. Now, for the specific implementation,

let me give you a brief overview. So let's assume the process happens like this. So here, suppose we implement a system call function called getHID. This is a function we implemented. It gets triggered from a task in user mode.

So once it's called, in the kernel, we actually assign each system call a system call number. This system call number is kept in the header file syscall.h. So our example here is quite simple. We've only defined one. After you go back,

you can extend it yourselves. Just put 1234 as an integer value, and that's fine. And then, look at how we implement this getpid, when we write this function, before we call ecall, we take this a7, this value, we take this system call number,

this number, this ID, and save it into a7. So this a7, is used to — essentially, it is used to pass the system call number. In the kernel, we check the value of this a7 With this register, we can tell which system call has occurred,

and then we just call the corresponding handler function. So after this ecall, we come in here, and once we're in here, we call equal, which we've already talked about earlier. Now, note one thing: when we call this function, in the end,

what we actually call is this, the TrapHandler function. And here, we've added a parameter, which is actually the exception occurrence, that an exception was triggered. So here we'll have many... earlier, there was actually a save...

register operations are essentially context save operations. Now, in the context save operation, note that it will actually save a0, a1, and a2 — it saves them into memory. Then, when we call the trap handler, we use up a0, a1, and a2

to pass some special values. Among them, over here, note that through a0 we pass in the context pointer, passing it in, because when we save, we actually store things in this memory region for the context. This MSquash of ours, as we mentioned earlier,

it is used to during task scheduling, or during exception handling, it is actually always pointing to it serves as a pointer, it actually points to that memory region for the context, when we save the context, don't we save it into here?

So this pointer always points to it. So once a2 is passed in, as you can imagine, we can, in this trap handler, inside this function, access what's in here. In here there's a0, the values from a all the way up to a5. Even though we know this value will be modified,

that's fine, because we've already saved it. Now the value in here holds our... the parameters passed during the system call. It's actually right here. The key is right here. So we need to take this mscratch, that is, the pointer to the trap context, and pass it in.

Okay. After passing it in, we go to the trap handler. You can see here, our third parameter here is a pointer to the trap context. We pass this parameter further down, into the system call, and then it will be used there. Now this doSyscall function,

It's actually one we implement inside our kernel — the general entry point for system calls. Now, inside this, its core logic is really just a bunch of switch-case statements, because it dispatches based on different system call numbers

to call different handler functions. Of course, it can be extended. There's only one here. We just kept it very simple here. And then, one thing to note, now that we have this, we can actually use this parameter to access values like a7,

as well as a0 and so on. Now, according to the earlier convention, these values are used to store the system call number is stored in here, and this one stores which is the first parameter and so on and so forth and they can all be used

and then, please note, that is to say, when we return the result, through the return value, we store it there, returned in a0. If it fails, it will return negative 1. Of course, what value does it return on failure? It depends on the specific error code,

which we define ourselves. In our case, we simply return negative 1. If it succeeds, it returns 0. So these values, like 0 and -1, when we return, they get saved in this context. Then, when we restore, we know there is a restore at this point,

right? During restore, those values are restored back. So when a0 goes back to user mode, it can get the successful result of the system call. You can tell whether the result is success or failure. This is doSysCall. This is the general entry point of our system calls.

Inside it, it dispatches to different implementations. Now here, what we focus on is, how the parameters are passed, how the parameters are passed, and how we follow this system call's This parameter convention, using these parameters,

using these registers, then finally, the return, the mret return there's nothing more to it. mret returns to this original place, and that's how we exit our system call. Now, in this process, the parameters are passed this way. What I've mainly

wanted to introduce to everyone here is this stuff. And so it comes back. Then, once it comes back, as you can imagine, our return value here this return is actually our a0 on this side so a0 gets it right here this is exactly what we talked about earlier

passing parameters for system calls so, and finally, there's what's called system call wrapping this idea, really is just something I want to mention to you all that is, in our current code, as you know, what we've actually done

is all pretty simple all the code is really wrapped into one part as the operating system so what we do is we take this The system call numbers, they're defined in a header file called syscode.h, and then, this function, the implementation of this function

is in the assembly file. So the actual logic that runs in kernel mode, that's wrapped up in this file, syscall.c. Now, in a real execution environment, like something such as Linux, under an operating system like that, it actually separates these pieces of code.

And the way it separates them, is actually quite simple: it takes this sys.h, this syscall.h header file, and splits it into two parts. So the left-hand part, It's actually, after you separate it out, it essentially becomes the C library we use all the time.

Now, this part is not actually distributed as part of the kernel. It's maintained separately. Then, at compile time, when compiling the application, it gets merged into the application. So the application will then call these kinds of functions,

basically the functions over here, like open, read, write, for example. After calling one of these functions, it will actually trigger an exception, trigger this exception. Now, the part on the right, this part on the right, is actually kept inside our kernel.

So what we'll see is that in the C library, its It has its own header file. This header file and the kernel's header file need to maintain a certain correspondence, because the two of them share some information, especially things like this,

information like system call numbers. Because if these values don't match up, then things will break on both sides, right? So on this side, let me just mention this to everyone. That is, if this were done properly, we should actually

pull this part out separately, because in a real industrial environment, this is actually provided in the form of a CQ, provided as a standard CQ. But for us here, we haven't made it that complicated. So actually, everything is put together

Pay attention to this, everyone Let me point this out So this code part, we can take a quick look at it That is, if I remove this, then after removing it, we know it actually goes through this piece of logic This piece of logic is just a demonstration

It actually makes a system call Now, the execution process of this logic is exactly what we talked about just now Let's take a quick look Now, after running it like this, we'll see the task still keeps running while our... Hold on a second

So this... Let me explain this execution process That is to say, task0 is created first. Looking at the code we just saw, task0 is created first. After it is created, it starts by calling this system call function. Now, as we know, once this system call

is triggered, it is actually an exception, and its exception cause value is 8, right? It is an exception triggered from user mode. So when this exception occurs, this exception, what it actually triggers is our system call. And our system call here

will be executed. What you see printed here, that is what this means. That's the idea. So, according to our understanding, that is to say, if this system call, after it finishes executing, then it should be able to return out. So we see

that here it does return. Now, why is it able to return? Everyone, pay attention here. It's because in exception handling, for this kind of system call exception, we make sure that its EPC value, we add 4 to it, we covered this earlier.

Otherwise, it would, it would go back into the ecall instruction again, running in an infinite loop. So that's why we can see this line. Returned so we know that it has actually returned from the system call again and at this point

its mode has switched back to user mode again and we can also use Hotlink to get the return value of that transfer at the same time, we can also use this parameter to fetch the value of course, what we pass on our side is in the form of a pointer

which also works because right now our memory basically hasn't been divided too strictly yet so in fact, this piece of memory can be accessed from both user mode and kernel mode so we actually just pass a pointer to get the data out

because our memory hasn't really been made strict like Linux's with strict user space comes with the memory protection so in practice, we can actually just pass pointers directly, which is fine just wanted to briefly mention that point

then after returning the task simply continues running so that is what a system call looks like in a simple example as for system calls, that's all we'll cover for now as for the related exercises, I hope everyone will go back and work on them

so that wraps up our introduction to system calls
