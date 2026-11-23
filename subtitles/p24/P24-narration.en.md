# English Narration Script

All right, let's continue today's class. Today we start on Chapter 14, called Task Synchronization and Locks. Earlier, we devoted quite a bit of space to walking you through everything from cooperative multitasking to preemptive multitasking, and all of that was really introducing

how, on our RISC-V-based platform, we can develop a multitasking relatively simple operating system. Now that we have multitasking, the next step is to look at some concepts related to tasks, including, for example, the concept of task synchronization.

The content of this chapter is presented in these four main parts. The first part covers the basic concepts of concurrency and synchronization. The second part will introduce how, within synchronization, we will walk you through

one synchronization method. it's basically the idea of a lock so some things related to locks, like critical sections, these concepts I'll introduce to you here. And then for the third part, we will introduce to you basically, for us,

locks can be implemented in many different ways. So among them, there's one called the spinlock, which is the main thing we'll introduce to you today. Chapter four, the fourth part, we'll then introduce to you some other Some synchronization methods

Now, mainly, I'll walk you through the key points. Alright, let's start by looking at this. This is the reference section. Reference articles. Let's begin with the first part. That is, some related concepts of concurrency and synchronization.

So first of all, we need to understand that is, what do we mean by concurrency? Concurrency, in English, is called Currency, Current meaning happening at the same time. The concept of happening at the same time. Now, here, this actually also involves

what we mentioned to you earlier A concept called control flow You might still remember Earlier we talked about We talked about something called control flow And also exceptional control flow Right? So this So now, when we look at this control flow,

we can actually think of it as what we discussed with you earlier, this notion of tasks, the notion of threads, they're all quite similar. So this concurrency, is simply about having multiple control flows that is, multiple tasks

executing at the same time However, Now, as for this control flow, there are actually many different situations. Let's break concurrency down into the following cases and go into a bit more detail for you. The first case is this: we should consider that,

as computers — sorry, as processors — have developed over time, it's very common nowadays that a single processor actually contains multiple cores, or that there are simply multiple processors. In that case, this is actually the most common

form of concurrent multitasking. As you can see up here, there are two — say, two processors, and on each processor, one control flow is running respectively. Between them, they are completely independent, with no interaction whatsoever

without any interference. Now, in modern computer architectures, this is called a multiprocessor architecture. Typically, under a multiprocessor architecture, memory is generally still shared. So, roughly, that's how it works. Now, in that case,

this is the most typical multitasking scenario on a multiprocessor. And there is another scenario, namely what's called multitasking on a single processor. This is actually the scenario on which our operating system development in this course is based,

that is, there is only one processor, but we still want to, on that single processor, run multiple control flows at the same time. So this thing is basically what we did earlier through scheduling, this kind of multitasking we implemented.

Two tasks go through the scheduler, meaning that for a while it schedules this one first, then for a while it schedules that one first. So they share one processor, and they share memory. Now in this situation, similarly, there are multiple control flows present,

so it can also be considered a form of concurrency. So besides this first case, actually there is another one we should talk about, in fact a very typical scenario, which is to consider the case of interrupts. An interrupt is also a kind of control flow,

right? That is to say, when an interrupt occurs, the interrupt and our ordinary task, can actually be considered as two, two kinds of concurrency. Two kinds of concurrency. So this kind here, that is to say, on a single-processor architecture,

a task plus an interrupt, between these two, there is actually a state of concurrency as well. This is also a situation we need to take into account, when we think about concurrency. So don't overlook this situation. Now, these three,

are what we call, for concurrency as a whole, we break it down into three states. When you think about concurrency issues, you really have to consider all three of these states. All of them. So that's the concept of concurrency.

The second thing we want to introduce is the concept of synchronization. So what about synchronization? Actually, it comes along with concurrency. When we deal with this concurrency, there is an approach we need to adopt — from a software perspective,

a programming technique that we call synchronization. So specifically, why do we need to add synchronization when we have concurrency? Let's continue and introduce that to you. Now, next, let's take a look at the concepts involved

when we use synchronization, such as critical sections, locks, and concepts like deadlock. Here, let's summarize them all together. First, let's take a look at what a critical section is. This concept, actually, when it comes to application programming,

there is a similar concept there as well. Think about it, isn't that right? When we are in a Linux environment, developing a multithreaded application program, we also bring in this concept of the critical section. I don't know if you still remember

whether the instructor introduced this concept to you in class. Well, the same thing actually, inside the kernel, this concept, well, it was actually proposed from the operating system's point of view. And of course, inside the operating system,

the same concept exists as well. So what is a critical section? Let's read through this together. That is to say, in this concurrent program execution environment, we've already introduced concurrency to you earlier. All three cases need to be considered.

Of course, for our current course, since we're based on a single processor, so the first case, multiprocessor concurrency, we can kind of set aside for now. But if you look at modern systems like Linux, they all support this kind of multiprocessor concurrency.

So that's a bit more complex. Now, in this concurrent program execution environment, what exactly is a critical section? First of all, we need to understand, a critical section itself refers to a fragment of instructions. Right here,

we've highlighted it for you in bold text, which is the so-called critical section. It actually refers to a sequence of instructions, a sequence of instructions. But, these instructions are, compared with ordinary instructions,

a little different, in that there is a characteristic that has to be met. That is, these instructions, what do they access? They access some shared resources. Now, as for these shared resources, there can be quite a variety of cases.

Depending on your actual situation, it could be a shared device, say a serial port, a screen, a display, or a shared memory region, some variables, some memory. That's roughly the idea. So, if, That is to say, if these instruction segments

access these shared resources, we call those instruction segments critical sections. Please pay attention to this — it's the basic concept of a critical section. At the same time, what we mean is, if an instruction segment accesses a shared resource,

it doesn't necessarily mean that it will expose one of our concurrency issues. So the critical sections we care about here, when it comes to critical sections, we actually also have to consider that, when multiple instruction segments

access this resource at the same time, that may trigger some problems. So for this kind of instruction sequence, that's when we pay special attention to it, and we call it a critical section. Now, this kind of why do we say this can cause problems?

Let me give you an example, so you can get a feel for the process. So suppose, look at the picture we've drawn here, there is this thing, that is, down below there is a processor, since we're using a single processor as our example to explain this to you,

so down below there's a processor, and the blue part up top, you can think of it as a shared resource, let's say it's a piece of memory, a variable, a variable in memory, and on the left, there is a control flow. instruction i, i+1, i+2

and they are executed sequentially. On the right side, there is also a control flow j, j+1, j+2, j+n, which is also executed sequentially there. Now suppose these two control flows, they both go and access this shared resource in the middle.

Sometimes they might produce some conflicts. Now suppose the control flow on the left, it actually acts on the variable inside, say this variable is an integer variable, and it keeps adding one to this integer variable, it may just keep adding one to it over and over,

while the right side is used to subtract one from it. So the logic on the left, in effect, just wants to keep adding, adding, adding, adding, until it reaches a certain number, and then it finishes. The right side wants to take an integer,

subtract, subtract, subtract, subtract, subtract. and when it's cut to a certain position, it ends. But think about it, actually, when these two control flows execute concurrently on one processor, what problem can actually arise?

It's that they may be executed in an interleaved way, right? Based on the concept of task scheduling we talked about earlier, we know that in a multitasking situation, when the task on the left is executing, it won't strictly, that is,

it won't proceed step by step without interference. When it reaches a certain state, perhaps because, for example, a timer interrupt occurs, our preemptive task will take this execution flow and switch it to its right side, to execute inside the task on the right.

So, on these two sides, the control flow... Because if they all go and access this block of memory above, then possibly during their execution, since this interrupt is here, if it keeps switching back and forth here, then the results they see may end up being different.

Think about it — isn't that the case? So if this kind of situation occurs, then under these kinds of conditions, we say that a sequence of instructions like this, we call it a critical section. Now, on the left, naturally, that is also a piece of code,

and it exists in a concurrent state, because the two of them, and the one on the right here, is actually also a critical section. The two of them, between each other, because after sharing a piece of memory, their operations on it,

may well affect each other. So this kind of interference can lead to some logical problems. And code like this is what we call a critical section. So when we discuss whether a piece of code is a critical section or not, it must be

in a situation like this, only then do we consider that code a critical section. Not all code is called a critical section. So that is the concept of a critical section that we need to understand first. Then how do we in these changing situations,

how do we control this task so it can effectively execute this critical section? Put plainly, when we need to execute these critical sections, how do we ensure that That is to say, for example, on the left, that critical section from just now,

its execution logic will not be affected by the handling of that critical section on the right, or be interfered with by it, right? So actually, what we say is that the most fundamental principle we adopt, in other words, when one control flow

enters a critical section, the other control flows must do what? They must wait. As long as we can ensure this problem is taken care of, then we can actually achieve this synchronization. To give an analogy, this critical section

Let me give a real-world example. Actually, you can think of it this way. That is to say, everyone can imagine that we... Because we say the concept of a critical section is a sequence of instructions, so it's really just a procedure,

step one, step two, step three, right? So let's take an example. Say we go to a bank ATM to withdraw money — a process like this, you can actually think of these steps of the process, as being a critical section, right? The first step of this procedure,

is that you first insert your card. After inserting the card, you enter the password, and after entering the password, you withdraw money, and after taking the money, you take the card back out, right? This sequence of steps,

Actually, we can think of it as a critical section. So, we say let's say I, when our person performs this operation, this person can actually be thought of as a thread or a control flow. So this person, he let's say I go first to

this ATM machine. This ATM machine can be thought of as you can think of it as a shared resource. So when we, our person, enters this, let's say I, let's say I get to this ATM machine first, and I'm performing this operation, then this process,

we say it actually cannot be logically, it cannot be interrupted. Because if you think about it, suppose I insert this card, and then after I enter the password, suddenly another thread or another controller comes along, and it starts operating on it as well,

then this part of mine, my subsequent I've basically given up this critical resource, so when I operate on it again afterwards, it's definitely going to cause problems, right? So what we actually need to do is how to this is basically the concept of concurrency,

so how do we make sure that this control flow of mine won't be interfered with? Actually, what we need to do is to make the second person, that is, the second control flow, He can't start his step. He has to wait until I finish this step first.

Then the other control flow can only wait there. So with this approach, we need to adopt a synchronization method to achieve this effect. Now, there are many ways to synchronize. The most fundamental, the most basic one, is to use locks.

A lock is the most common technique used to implement synchronization. Now let's look at locks. There are many kinds, as we said. They include non-sleepable locks and sleepable locks. Now this concept, what it actually refers to is this:

because whoever holds the lock, well, actually, whoever executes the lock, is certainly a control flow. And a control flow, so you can think of it as just one task now for this task what it means by non-sleepable actually refers to

it cannot leave our scheduling that is, leave but this so-called concept of sleeping actually when we implemented multitasking earlier we didn't introduce it we actually left it as a later exercise for you all to work on because the so-called non-sleepable

is equivalent to saying the task that holds this lock cannot leave our scheduling queue cannot exit our scheduling while sleepable is equivalent to saying after taking the lock we can let this task enter a state where it does not participate in scheduling

But right now, what we're doing here is, since we don't want to bring in too much of the whole notion of sleeping, what I'm going to present to you focuses on, and from here on it's mainly about introducing a kind of non-sleepable lock.

So it's the non-sleepable lock, as the example, that I'll be walking you through. Okay. So this, let's take a look at this, implementing a lock, and how it achieves this thing we call the synchronization effect. So over here, Let's look at this animation

to demonstrate to you how a lock actually achieves this synchronization. So again, let's use the same example as before. You can see that down here is a processor, and the blue part up here is some shared memory. On the left is one instruction stream,

and on the right is another instruction stream. So how do we implement a lock? First, we need to create a lock. You can first think of it logically— we can assume we need to create a lock. Now, this lock, at the very beginning,

we can initialize it to the unlocked state, which is like saying if we go back to that money withdrawal example from earlier, So that's basically like saying we can be outside the ATM machine we can all imagine it like this there is a room

and on the door of that room there is a lock and this lock is initially in an unlocked state in an unlocked state So this so this thing basically means we need to, in this just having this lock is not enough what we actually also need to do is

we still need to, in the execution flow we just had that is, before and after our execution steps we need to add what is called one called acquire which is the action of acquiring the lock And then, after this action is finished,

we need to add an action to release the lock. So release, and acquire, we often also correspond them to, that is, acquire is called lock, and release is called unlock. So acquire, you can think of it as locking, and release is unlocking.

So, in other words, taking the withdrawal example from earlier, still using that as an example, that is to say, it's equivalent to saying, before we enter the ATM room, and before we insert the card and start the operation, our first step is to close the lock.

And once it's closed, you can imagine that, Then the other person simply can't get in. Think about it, isn't that right? So it's basically like this. So before each person operates on their step, they have to first try to acquire this lock,

and then, after they leave, they just release the lock, and that's it. So with the lock added, let's see how this achieves the synchronization effect. As we mentioned earlier, synchronization means making another task wait. Remember this key point:

you make another task wait. Waiting can take many forms. It can be sleeping, or it can be non-sleeping. Our focus will be on the non-sleeping approach as our example. Okay. Now suppose the control flow on the left, the task on the left,

first gets the CPU's execution rights. So the PC, first points to this execution flow on the left. And the PC, according to our current logic, as we know, the PC, doesn't start working right away. First it has to try to acquire this lock.

Then, after acquire is executed, since this lock right now, is in an open state, that is, a free state, so when our task on the left, goes and does the acquire, as you can imagine, the state of this lock will naturally change. It will become the locked state.

It's like this door has just been closed. Right? So the task on the left, can now go ahead and execute its steps with confidence. The PC just starts moving downward as it executes. Now, during the execution, as you can imagine,

suppose we are still in this single-processor multitasking scenario, then an interrupt could happen. So what does it mean when an interrupt occurs? It means our task is going to be switched out. Right? So once a switch happens,

as you can imagine, this PC pointer will go from the left side, the gray part. The gray color here means, it's basically like the multitasking we did earlier. As you know, at this point the PC effectively remembers, the position to return to.

It remembers the return position. So once this interrupt happens, the task on the left essentially leaves the CPU, and the task on the right gets the CPU. Now, for the task on the right, once its control flow gets the CPU, you know that the very first operation it's going to perform

is definitely to try to acquire this lock, right? Now at this point, as you know, since the current state of the lock is locked, you can imagine that its attempt to acquire, to take the lock, is definitely going to fail. So since it fails,

the effect we want to achieve, is to make the task on the right wait, and not proceed any further. Note carefully, that is to say, what we mean by waiting In fact, it means it can't proceed further. Because once it does execute,

then those two execution flows would be running concurrently, which would cause conflicts. Now, as for this, this so-called making it wait, exactly what mechanism is used to make it wait? Well, there are many ways to do it. For now, let's set that aside.

At least we know, that is, at this general conceptual level, the task on the right basically means, it can no longer move forward. So, since the task on the right can't move forward, then as you all know, what our entire CPU does

is still to keep going forward. The CPU is still executing instructions one by one. But it's like the instruction on the right here it will actually be executed repeatedly which is exactly what we'll talk about later, this spinlock,

which is really designed to achieve this effect Now, let's not worry about that let's just say the task on the right is basically, well, as you know, it definitely can't keep running it might be sleeping, it has left the scheduler

Then naturally, after the new interrupt happens, our scheduling might come back around again What I mean by coming back is that the task on the left may once again get permission to execute on the CPU and then it starts running again

Alright? and it just continues executing from where it was So as you can see at this point, its so-called non-preemptible the instructions inside its critical section, even though we know the task on the right will actually still come and preempt it,

but you know, even though it preempts, it won't actually because the task on the right won't continue executing, so it really won't interfere with it. So the instructions of the task on the left, in effect, can proceed without interference, logically speaking.

Please take note of this — it's especially important. Now there's another point to keep in mind: it's possible that because interrupts keep happening, so during the execution of the task on the left, the task on its right may still preempt it again,

this handling privilege. So this, so when we talk about So this critical section on the left not being able to run without being interrupted, that does not mean the task on the right won't take part in scheduling. Instead, it may still

because as long as it doesn't sleep, it will still be scheduled in. So this task on the right will still preempt the CPU, and execute. But as you can imagine, at this point it still can't make any further progress, it can't make any progress.

And that achieves our synchronization effect here, keeping the task on the right that is, equivalent to saying he's going to be waiting here waiting only on the lock It's like a person outside a door he just has to wait; he can't get in

So, on this point, I hope everyone, when understanding this synchronization and this concurrency, synchronization and locks, will keep this in mind. Right. So as you can imagine, in short, the task on the right, since it has no chance

to execute the code in its critical section, then the task on the left, the one on the left, even though it may be preempted, it can still, logically, guarantee that its... The instructions in the critical section can be safely

logically safe will be when we say it is logically safe logically it can still be interrupted in practice please note this in practice it can still be interrupted so it is logically safe running all the way to the end but in between there can still be interruptions

this is something you also need to pay attention to when understanding this multitasking so this, so finally because there is a release process the release process so as you can imagine once it is released then this lock will naturally be released

Finally, this process of ours will still be released. So once it is released, then this lock of ours, when the control flow on the right is executing, it will see that this lock is effectively in the unlocked state. So at this point, as you know,

once an interrupt occurs, and the task on the right gets control, its acquired operation will actually succeed, unlike before, when it would be blocked here. So at this point, it is as if the task on the right has acquired this lock again.

After acquiring this lock, it can continue executing sequentially, executing all the way until the execution on its right executes successfully So this entire process is actually as you can see, throughout this whole process, the two tasks achieve

this synchronization effect. This is what we mean by introducing locks to achieve synchronization, the internal low-level implementation process. Now, after introducing this concept of locks, when you program from now on, you will probably

need to pay special attention to certain issues. For example, for example, we often run into what is called the deadlock problem. Deadlock is called deadlock So what exactly is a deadlock? Here's the basic definition. When, along the execution path of a control flow,

multiple locks are involved, the example we just solved was still fairly simple, with just one lock, for instance. But as our logic grows more and more complex, there will be more and more cases of synchronization. Under these more complicated situations,

we may need to define multiple locks, to synchronize different shared resources and critical sections. So at this point, what we need to pay attention to is, a deadlock definitely occurs in situations where multiple locks exist.

If the order in which these locks are acquired -- that acquisition order -- is not handled carefully enough, a deadlock problem can easily happen. Let's take a simple example. Now let's get a feel for what a deadlock is. That is to say,

suppose we now have two locks, two locks, A and B. Now on the left there is still one execution control flow, and on the right there is also an execution control flow. So the control flow on the left, its order of acquiring locks is basically to acquire A first,

and then go on to try to acquire B. On the right, the order is to first try to acquire B, and then try to acquire A. Now in this situation, it is possible for note that I said it is possible, meaning it is not absolutely going to happen.

The example we are giving here, of course, is a state where a deadlock could possibly occur, it could possibly happen. So let's take a look at why this could possibly happen. Actually, this process is still very simple. Let me give you a quick demonstration.

So, look at the left side, suppose this task on the left first gets the execution rights of the CPU. Then it first tries to acquire the first lock, A. And after it gets this lock, well, it may be in its scheduling process get scheduled again,

and B gets it. Then B starts executing, since it first tries to acquire B, the second task gets the lock B. Now you can imagine that it's possible this task gets switched back again. After it's switched back, at this point, The task on the left

when it tries to acquire B, then, as you can imagine, since this lock B has already been taken by the task on the right, already been taken by the task on the right, so there is no way it can get this lock. Right? So this lock is definitely not acquirable.

Then, since it cannot be acquired, the task on the left is essentially stuck here, at this instruction, it will actually be waiting, waiting at this point. Now at this point, if the task switches back and B tries to acquire A,

then B is here, because for A, it had released this lock A earlier, but it did not go ahead and acquire this lock A first, but it did not release it either. Then the lock on the right, the task on the right tries again, and maybe A naturally

can't proceed either. So in this situation, what kind of situation arises? That is to say, if the task switches over now, switches to the left, the left task certainly can't move forward either. And then, even if it goes back to the right,

the right can't proceed either. So the two tasks end up both waiting for each other to release the lock the other one holds, unable to make any progress. So under these conditions, this is a very typical deadlock situation— a state of deadlock.

So as everyone can imagine, these two tasks actually have no way to proceed any further. This is a deadlock. So this is the simplest case of a deadlock occurring. Now, with this kind of deadlock, how do we solve it? Well, there are some principles for solving it,

There are still some. and I'll briefly go over them with you. That is to say, first of all, we can adjust the order in which locks are acquired. For instance, keeping it consistent. What does that mean? Well, still taking the example above,

you can imagine if the locks on both sides Just now, the problem we ran into, as you can see, is that in fact, the reason this problem occurred, is simply because the order in which they acquire locks is inconsistent. On the left, it's A first, then B,

and on the right, it's B first, then A. So if we take this order, and swap it around, if both sides do A first, then B, the problem basically won't occur anymore. It basically won't occur anymore. You can go back and try this yourselves.

It's really easy to make this comparison. Then the second way to solve the deadlock problem, is to prevent a task as much as possible from requesting another lock while already holding one lock. Now, since we saw this problem,

one possible situation where the earlier problem exists So, for example, take the task on the left, it first acquires lock A, doesn't release it, and then goes and tries to grab another lock. Now that could potentially cause problems.

See what I mean? So what should we do about it? Well, at design time, we can, that is to say, there are quite a few approaches. One is, for instance, a fairly greedy approach: assume we have multiple locks, then when I from a task's perspective,

when I acquire the locks, I can atomically request all of these locks in one shot. Then in fact, if they are all For example, if I acquire all of these locks, and then, after I finish my work, I release all of them at once, then naturally, other people

won't end up competing with anyone else, and this naturally avoids the problem of mutual deadlock. Think about it, isn't that right? But this situation is actually somewhat well, it's a rather extreme case, because this is a bit excessive.

It's basically like grabbing all the resources in your hands at once, but you might not immediately be able to use them all up right away. So this approach isn't really appropriate either. So what's a better way? Perhaps it's to say

what we should do is when we're programming, What you can do is this: whichever locks you use, you only request those locks, and then, instead of requesting the next lock, or rather, when you request a lock, that is essentially requesting a shared resource.

So when you request the next shared resource, you should release whatever you requested before as much as possible. In other words, don't hold on to one thing while grabbing another. Doing that can also avoid this kind of effectively avoid this deadlock problem.

That's the second approach to solving deadlock. Now the third one is, of course, more extreme: use locks as little as possible. That is, when you're programming, don't get carried away thinking concurrency is just so convenient,

just because concurrency can indeed improve efficiency. So in practice, when programming, Actually, we should start from the simplest angle as much as possible use less of this concurrency use fewer multithreads then naturally we'll use fewer locks

and there will be less concurrency and naturally it won't lead to those annoying deadlock problems because once deadlock problems occur debugging becomes very troublesome So if it can be simple keep it as simple as possible This is also one of our main

programming principles that is, right from the start don't make the code too fancy In other words, the simpler you can make it just use the simplest approach to solve the most complex problems is the best kind of strategy So these are some

concepts related to deadlock. So next, let's actually take a look at one kind of since ultimately we need to implement in code these concepts of synchronization and concurrency. So let's take a look at the specific implementation we adopt,

which is the concept of the spin lock. In our course for now, we'll use the spin lock as a representative to introduce this to you. As we discussed earlier, locks fall into two typical types of locks: one is the non-sleepable kind,

One type is sleepable, and then there's what's called non-sleepable. As I explained just now, that is, when it holds this lock, the task does not drop out of scheduling. It still takes part in scheduling. As for the so-called sleepable lock,

that means when we hold this lock, we will take the task out of scheduling. It won't take part in scheduling. Now the spinlock we're talking about here, it actually falls under the typical non-sleepable locks. That means that

when we go and acquire this lock, our task still takes part in scheduling. It still takes part in scheduling. So as for the spinlock, its English name, is spinlock, which literally means spinning. spin means to spin, like a spiral

That's the concept of spinning So why is it called spinning? Let's take a look at its actual implementation It's still very typical So here is a classic example of implementing a spin lock A typical example As you can see First, we can define a struct for a spin lock

Inside this struct we define something that essentially represents a spin lock And this lock has only one member which is a you can think of it simply as a flag If this flag, this value, is 0, it means this lock is in the open state

it is not locked When the value becomes 1, the lock is essentially locked This is the definition of one of its structs Now, the second one What follows below are actually some functions for operating on this lock Since we've studied data structures

you should find these patterns quite familiar That is, you first define a struct and then, around that struct, you design some related operations There's even a little bit of object-oriented thinking here, isn't there? So, this one

has three basic operations which are basically its API One of them is initialization Now, for initialization, we say that this lock, when we initialize it, we always put it in an unlocked state And then, as for the spin lock, It actually corresponds to

what we talked about earlier the concept of acquiring that is, trying to get hold of this lock trying to lock it and unlock, that's releasing this lock trying to open this lock and giving back the right to use it Now, the key point is

this so-called spinning concept actually comes from the locking operation in that locking operation, what kind of concept do you see there? it has a loop inside and this loop, you can see it's an infinite loop So, inside this infinite loop,

What does it do? Well, once it comes in, it first checks whether this lock is already locked. So if it equals 0, that means the lock is not yet taken. If it is not locked, then we grab this lock. Once we have grabbed it, we lock it up,

simply setting it to 1. Now that it is locked, we break out of this loop. Now, if you put this together with the logic from earlier, you can see that right after acquired, after this process, after this step, comes our critical section,

the operation we want to perform. So what this means is, Once we get this lock and lock it, we actually jump out of this loop, and then we can execute the instructions in the critical section below. Conversely, if this lock, that is, when we go to acquire it,

we find that this lock is already locked, then as you can imagine, this logic will come back and keep trying to check, keep trying to acquire this lock. So, so the task that hasn't gotten this lock, that task, as you can imagine at this point,

it will keep entering a state of In a loop, it keeps trying to acquire this lock, and this continuous trying process naturally blocks the task in front of the critical section. This is exactly the effect we just described, where the task simply cannot proceed any further.

Now, this spinning process, we call it spinning, because it is spinning by itself, rather than being spun by someone else, right? That is the concept of a spinlock. Now, over here, building on the example we just talked about, let's look at a

case where, once we have these interfaces, how we would roughly use them in our programming. So first of all, we need to define a lock variable object. Then, we initialize it. There's nothing much to say about that. At the same time,

please note something, that is, for the two control flows, since both control flows need to see this lock, this lock, the memory where this variable resides, should also be shared between the two control flows. Now, once we have this lock,

each control flow, before and after its critical section, adds a lock and unlock respectively. So as you can imagine, based on the pseudo-code, as we just explained to you, if the task on the left acquires this lock first, then when the one on the right comes in,

even though it is still scheduled, since we said this is a non-sleepable lock, the task on the right will still These two tasks will actually keep getting scheduled So if the one on the right gets the lock first, or if the one on the left gets the lock first,

then when the right one tries, based on what we just explained, it will actually end up spinning. And while it's spinning, the logic below just can't proceed. That means even if it gets scheduled, takes part in scheduling and execution,

it still can't move forward. So what we achieve is letting the task on the left safely exit the critical section, finish executing the critical section. So this is really a kind of so-called synchronous waiting result. But this one,

we're just using this example to introduce to you what's called spinning. But actually, this piece of code, this spinning code, actually has some issues. That is to say, we need to introduce to you next, that is, if we simply adopt this approach

to implement spinning, there will actually be problems. So why are there problems? As for this problem, if we look at it from the C source code, we actually can't see it very clearly. But if we disassemble this piece of code, and actually look at its assembly code,

we will gain a deeper understanding of why simply checking and locking like this can actually cause some problems. So let's take a look at the code generated after disassembly. Now, the code generated after disassembly actually looks like this.

This code and the code on the left can actually be mapped to each other to some extent. First, there's an LW operation here, and as we mentioned earlier, LW essentially takes data from where? It loads data from memory into a register.

So what it actually does is take some data from memory and load it into the a5 register. Now, what exactly is being loaded? It's loading this lock, because as we know, the lock variable we defined is actually in memory. It takes the value stored in it

and loads it into a5. So what's in a5 is essentially our LK variable's lock member, its value. So next, it's actually doing a check, that is, it checks whether the value in a5 is equal to 0. So the logic it checks here is actually the same.

If it's not equal, that is, if it's not equal to 0, then it actually goes back and loops. Let me take a look. Right. If it's not equal, meaning it's not equal to 0, then it goes back and loops. If it equals 0, then it takes that A4 is actually

1 and then it takes that 1 and writes it back into here so it achieves this effect the effect of negating and then adding 1 So... this is just to give it a brief explanation of how this assembly maps to the code on the left But in this case,

as we said, if both control flows might go and execute this logic, then in fact, it introduces a problem. What kind of problem? That is, it actually can't block to achieve this synchronization effect. Let's assume, suppose we can give an example now.

This one, the reason is, you all must deeply understand, that is to say, when we do task scheduling, our so-called interrupt occurrence, interruption, and preemption, in fact, what it preempts is at the assembly, that is, machine instruction level,

and you can't simply think of it as at the C language level, this kind of high-level language level. So each machine instruction up there, any one of them can be interrupted. Now, think about it, suppose the control flow on the left,

suppose it gets the CPU first. First, it starts executing When execution reaches this logic, it also sees that the value of this lock is equal to 0. That is, suppose the lock is open at the beginning, then if it is equal to 0,

it will still interpret that this lock is not being held or contended. But suppose that at this moment, the control flow on the left gets preempted, and after it is preempted, the control flow on the right starts executing. Now the control flow on the right,

because when the control flow on the left was preempted, it hadn't had a chance to execute the logic below yet, that is to say, the control flow on the left hadn't gotten to acquire the lock yet, then the control flow on the right might get the CPU again,

and at that point it also sees that the lock is not locked, so it's possible that both control flows actually determine that the lock hasn't been acquired yet, which leads to both of these control flows and it might skip this locking process,

skip this, this spinning process, and go straight in to after repeatedly locking this lock, after locking it again and again, it ends up going into the logic of the critical section down below. Think about it, isn't that right?

So in that case, this... So the actual problem with it, that is to say, it's essentially like saying, what kind of problem exists here? The point is, if we do it this way, we actually cannot achieve the effect of synchronizing the control flow.

And the root cause, Actually, it's still because when we say it uses this kind of simple atomic and logic in the operation of reading the lock's state and actually acquiring the lock, it is not completed within a single instruction.

And for us to guarantee that this thing can execute correctly, we actually have to ensure that reading the lock's state and acquiring the lock are done atomically, only then can the locking actually be realized. So if we just write it simply like this,

we actually cannot achieve the intended effect of a spinlock. On this point, based on what I just said, that is, the example, That is to say, if we break this action down at the machine instruction level, then at every single machine instruction,

there is a chance of being interrupted at any one of those instructions. And once that interruption happens, it's possible that, I mean, both sides of the control flow wrongly assume that this lock is in an unlocked state. What example can we use to understand this?

Well, you can imagine, that is to say, it's like, the first person sees that this lock is open, and logically they should immediately lock this lock right away, so as to prevent another person from coming in. Think about it, isn't that right?

Now, imagine, very unfortunately, that right after he saw that this lock was in the unlocked state, he just say he suddenly lost consciousness, suppose the behavior of not knowing, not too strong, he just suddenly lost consciousness.

Then the second person also comes up and, seeing that this lock is also unlocked, so effectively both people believe that this lock is unlocked. Then the second person he might first just grab this lock first. So this the first person, he might

when he wakes up, since he still remembers the state he last saw was unlocked, at this point, he just continues on, and if he goes to acquire the lock again, he then repeatedly puts the lock back on, so the two people, in effect,

it's like saying, in reality, we haven't achieved this protective effect. So, we how do we solve this problem? Well, this thing, using ordinary instructions, can't actually be done. So when we actually implement it, we need to introduce some more complex atomic instructions,

so that actions that normally would take multiple steps with ordinary instructions, we can complete atomically in a single step. Now, for this kind of thing, we say that at the architecture level, it actually provides this kind of

support — it has taken this problem into account. So there are special instructions that help us implement it. Now, at our upper level, since we program in C, we can actually call this function, namely the function SyncLockerTestAndSet.

Now, as for this function, it actually lives in it is really a C function wrapped by GCC. Now, if we actually take this piece of code and disassemble it, you can see It actually has a special instruction inside. And that's this instruction right here.

It's called the AMO Swap instruction. Now, with this instruction, what effect does it actually achieve? Well, it's what we mentioned earlier. It can fetch the state of this lock, and perform the locking action, so that both happen as

one atomic step — completed within a single instruction. So this kind of thing falls under atomic operation instructions, and in fact they are also provided in our RISC-V instruction set. If you're interested, you can take a look.

Volume 1 of the spec, the one on unprivileged instructions, has a chapter on the A — the atomic extension instruction set. It defines some atomic operation instructions that go beyond the basic integer instructions. atomic operation instructions

Now, the format of this instruction is roughly like this. So, this effect How does it roughly achieve this effect? Let me briefly explain it to you. That is to say, this way, we can achieve the effect we just saw. So how does it do that?

Let's take a look. Suppose this assembly code, let me briefly walk you through it. As for this assembly code, what is the front part doing? Actually, it takes the effect of a4, which is essentially, the a4 register, it actually holds a,

this corresponds to this thing, it's this thing. In the a4 register, it essentially holds the address of the lock member variable of this lock a4 you can think of a4 as holding an address this address points to the lock member of this variable

and then the second one, a5, li a5 comma 1 as we discussed earlier li is essentially load immediate right? so the effect of the second instruction is putting the value 1 into the a5 register that is to say you can think of it this way:

these two registers this one is a4 this one is a5 so as for a4, Actually it is a pointer We placed a pointer This pointer points to this lock's this lock variable, this block of memory. Then in a5, we put an initial value of 1. And then the next one,

is our key instruction here. It is called AMO. AMO is actually the abbreviation for atomic memory operation, that is, atomic memory operation instructions. Now here, it has swap appended to it, which means this instruction is actually doing a swap.

So what kind of swap is it doing? It is actually swapping the value in the memory pointed to by a5 and the value in a4, exchanging them. What does that mean? It means it takes this A... Actually, let me put it this way. It works like this:

It does a swap like this. It takes the value in the memory pointed to by a4, and swaps it with the a5 register. That is, it puts E into that location, replacing the original value, and then takes the value that was in there and puts it into a5.

That's the effect it achieves. Now, the logic in the next part is really checking the value in a5. It first copies a5 into a3, and then checks whether the value in a3 is equal to 0. So what it's actually checking is whether the value in here is equal to 0.

If it is equal to 0, then what does that tell us? That is to say, the original value inside this lock was 0. Which means it was not locked. Not locked. Then it will actually just continue executing. If the original value in here was 1,

then it is locked. Then it will enter the loop. That is, if it is not equal to 0, then it loops. Right? If it is not equal to 0, it loops. So if the original value inside was 1, it means someone else has already locked it. Then I can only go and spin.

Now, note one thing. It actually uses a little trick here. That is, when it does the swap, it swaps a 1 into it. So what effect does that achieve? That is to say, if this lock was already locked, If it was already 1, then it actually doesn't change the value inside.

Right? But if it was originally just 0 inside, meaning it wasn't locked, then as soon as it puts 1 into it, that's essentially the same as locking it. Think about it, isn't that right? So, you can imagine that for a if it starts out in an unlocked state,

then it's essentially like saying within a single instruction, it completes the locking and fetches the value from inside to make a judgment later on, all in one action. That's the atomic instruction, and this is where its real power lies.

In a single instruction, and that completes this operation then we won't get the kind of separated state we mentioned earlier So, all it takes is it's as if when I see this lock in this state, I just grab this lock and first, no matter what,

I lock it up right away then no one else can race against me and that's the effect we achieve Now, this kind of is the effect of atomic operations So when we implement a spinlock, we actually need to make full use of these atomic operation instructions

in order to achieve the effect we want Otherwise, it just can't be done. Please pay attention to this. All right. So this, is what I want to walk you through. This kind of spinlock implementation, actually, by the way, also introduces to you

the atomic operation instructions in RISC-V. For the related content, you can go look it up yourselves, in the relevant chapters of Reference 1. Now, coming back to our actual example, in our real internal implementation, it's actually very simple,

because we adopted a somewhat clever shortcut. So what did we actually do? We actually well, think about it, why did we run into this so-called problem of concurrent preemption in the first place? and especially in this our current premise is this single-processor environment

actually we can see the main problem really comes from what happens after an interrupt which causes preemption, right? So if we consider our precondition being just a single processor a single processor like this, then our locking acquire logic

can actually be made very simple. How do we do that? We just need to turn off interrupts. This is what I'm demonstrating to you here, that is, one way we implemented it, the simplest approach, which is to turn interrupts off. How do we do it?

We just turn interrupts off. And once interrupts are turned off, you can imagine what happens next. In fact, a second task won't even be created. It just won't be switched over to. In other words, any task... if the task on the left grabs the lock first,

then inside this critical section, it disables interrupts. Once interrupts are disabled, other tasks have no chance to participate at all, because scheduling no longer happens. Remember, our scheduling is driven by interrupts.

Right? So if scheduling doesn't happen, then naturally nothing will preempt you. And that naturally guarantees that the critical section on the left is logically never interrupted. So what we actually implement here is using disabling interrupts to achieve this.

Please pay attention to this point. Let's take a look at the actual code. So for the code, we'll go straight into our OS code. Today's should correspond to our number 09. It corresponds to today's number... The one just now was chapter 14.

And here, in the examples, it's 09. The 09 lock part. Now, the code for this lock, it's all concentrated in a single file, called lock.c. So this lock.c, as you can see, our spinlock, the way it's implemented, is actually very simple.

It just disables interrupts in machine mode. And what it operates on is the mstatus register. This register, we've actually mentioned it to you before. So when unlocking, we simply restore and re-enable the interrupts. Now, this is a very simple and brute-force approach,

but in this particular scenario of ours, it can achieve a similar effect. So let's take a look, and demonstrate what happens with interrupts after locking them. So when doing the experiment, you can look directly at our user.c.

In there, we've defined two tasks. Now in the first task, we have a section that uses a spinlock. We've defined a macro here for whether to use it or not. If we remove this macro, then this lock, in effect, Then we just don't add it.

So if this is defined, we add the lock. Now, what effect are we trying to achieve? That is to say, as you can see, we have a piece of logic like this. Inside this logic, in this task0, after task0 prints this begin, we do this. We make this printing in task0,

execute five times. Execute five times. And for those five times, we print a begin at the front, and an end at the back. So we consider this part, the critical section. This piece of logic is called the critical section. Now this spinlock of ours,

At the beginning, we acquire this lock. At the end, we release this lock. So what effect do we achieve? The idea is that we want, for the code in this critical section, once the lock is acquired, to guarantee that the five-iteration loop below,

can output continuously, without being interrupted by other tasks in between. So what if there is no lock? As you can imagine, that goal cannot be achieved. Right? Let's remove this lock first. Let's see what the effect looks like.

It was 09 just now, right? Let's go down and compile it. So right now we have no lock. Without the lock, what kind of effect will we see? Actually, the effect is already quite obvious. I just stopped it there. So what can we see?

That is to say, task0 is created first. task0 starts running. This printing got a bit garbled. Now, this running message, after it was printed just once, we see that as soon as an interrupt occurs, the tasks actually get switched.

Now, because there's no lock, task1 starts running. So this 'running' output from task0 basically gets interrupted. So this task0 of ours, it cannot be guaranteed to output continuously. We cannot guarantee continuous output. And task1 will keep interrupting it in the middle.

So if we want to achieve this, The critical section can continuously be logically not interrupted. Then we open this lock. So based on what we just explained, this actually disables interrupts inside. Once interrupts are disabled,

it is essentially equivalent to saying that this other task has no chance of being scheduled at all. So naturally we achieve this effect. Let's take a look at whether it really works exactly as we imagined. As you can see, look at this Task 0.

Even though every time it prints it gets delayed for a long time, it still... Task 1 at this point cannot get in at all. Only when this Task 0 has printed five times, The moment it is pressed, at this point, because at this point,

we release the lock again. Once the lock is released, after the tasks come in, Task 1 you can see that now they start coming in again, able to get in. But as soon as Task 0 gets scheduled in and starts running, then you can see the other tasks

actually have no way to interrupt it. So these five iterations will always run together, and that is the critical section. The critical section can to run safely. So nothing else can sneak in between. Then logically, we can get a certain guarantee.

That's the concept of synchronization. This example is very simple. After you go back, you can try it out and feel it for yourselves. So this is the spin lock. The spin lock. Of course, the approach we adopted is really just one

of the ways to implement a spin lock — a rather crude implementation, just directly turning interrupts off. We don't even spin. But whether it's really spinning or this kind of approach, when we talk about designing the use of spin locks,

it still has some limitations of course, it has its advantages the advantage is its efficiency is quite high because the task doesn't actually exit exit the scheduling so it can come back in at any time now, this however, it also has some problems

as we have also seen that is to say, first of all, after it acquires this spinlock, then, inside the critical section, we actually do not recommend performing any long-running operations so, like what we just did in that example,

those kinds of approaches are really not recommended and in particular, that is to say In normal code, in the real world, a spinlock is actually, because of its particular properties, it can indeed also be used in, for example, interrupt paths.

So you can imagine that if, in an interrupt path, we were to perform some long-running operation, then after using this so-called protection, that effectively means other tasks cannot get executed. So once this interrupt blocks in there for a long time,

that is definitely not appropriate, right? In particular, inside a critical section protected by a spinlock, you also cannot voluntarily give up the CPU. That effectively means This task is actually not allowed to sleep. Although we didn't before...

we didn't demonstrate sleeping earlier. But the so-called sleeping... as we explained before... it means you cannot give up the scheduler... drop out of scheduling. Now, because you can imagine... that is, if you're inside a spin lock...

once you've acquired the lock... and then you give up scheduling... then it's very likely... you yourself will have no way... because once you voluntarily give up the CPU... whether you can come back... is actually questionable.

Suppose there's a problem in your program design... and you never come back... then this lock can never be released. And if it can never be released... Then you could end up bringing down the whole system. So, that's why we say after adopting spinlocks,

although their advantages are quite obvious, the critical sections we actually protect with spinlocks, in other words, you can only run some quick logic inside them. Once you acquire the lock, execute very quickly, and then release the lock right away.

You must not, inside the lock's critical section, perform long-running operations, or even go to sleep. None of that is allowed. In real systems, in Linux, for example, when spinlocks are used, this is something we need to pay special attention to.

So, as for spinlocks, that's all we'll cover in this brief introduction. Now, this course of ours, since it's fairly short, Actually, the main goal is to give you a feel for some basic concepts of synchronization and concurrency,

and the simplest use of spin locks. Now, of course, there are many other synchronization techniques. Here, since time is limited, we won't go through them one by one. We've just put together a simple table for you here. If you're interested,

there's actually a lot of material you can look at. So, for example, besides spin locks, we also have the most commonly used ones, such as semaphores. Now, with semaphores, we say they support sleeping, meaning that when you hold the semaphore,

tasks inside your critical section are allowed to sleep. Then there are also some other things, like we also frequently use, for example, mutexes. Now, a mutex, actually, in real practice, Actually, this is the simplest form of a semaphore.

So this... There are also some others, like completion variables and so on. Waking up tasks through signals, and so on. So there are quite a few other synchronization techniques. We won't go into them in detail here. Through this chapter,

the main thing is to understand some basic concepts, concepts related to synchronization, and locking. As for the related exercises, you can go back and try them out yourselves.
