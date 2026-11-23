# English Narration Script

Today we'll continue with Chapter 9.

So from this chapter on, we'll slowly start introducing, that is, when we implement a small operating system, the concept of multitasking, or some of the ready-made concepts you've encountered in Linux, will gradually be brought in here.

So in Chapter 9, we'll first look at a simple way to implement cooperative multitasking. So this part mainly first introduces to you the concept of multitasking and the concept of context. Then in the second part, I'll briefly show you

how we can, based on this RiskFile, this simulated device here, implement a simple cooperative multitasking. Mainly these two parts. So let's first look at some concepts, like what is multitasking, and context. So first let's look at the so-called concept of a task.

As you know, what we're in contact with now, this diagram, actually shows that When we execute code, this function gets called over and over again. So no matter how function A calls function B, and including when it finally returns,

you can see that the whole execution process is actually serial, a single stream of instruction execution, right? Now, in the course of this instruction execution stream, we use some concepts involving the stack. This is what we call a...

Actually, what we call a task, you can think of it as simply the execution process of a function, a stream of instruction execution. That's the simple concept of a task. Now, what about multi-tasking? Just imagine, suppose we have multiple cores,

and each core is running a function. and its subfunctions. So when multiple execution flows are running at the same time, that's actually the simplest form of multitasking. Of course, this requires multiple cores executing simultaneously.

If there are two cores, you have two tasks running at the same time. Now sometimes we talk about this hardware, we say the number of cores is always the number of harts is always limited. For example, in the system we're implementing now,

we actually only have one core. So since we only have one core, but we still want to run multiple tasks on one core, to be able to achieve multitasking, what do we do? That's actually exactly what we're going to work on. That is, right now we only have one core.

By the way, let me mention this, in the exercises, you should have also seen That is, when we run it, when we actually start up QEMU, we have a parameter here, -smp 1, and what that does, basically, is to take our number of cores at startup

and limit it to just one. But actually, as we said, this virtual machine actually supports this machine, this virtual machine, actually supports up to 8 cores. So you could actually pass in 8 here. But with 8, we won't actually have all 8 running at the same time.

This is covered in our first exercise, the bootstrap exercise, because, as you can already see, we take the other seven and actually suspend them. So what we do later, in fact, is implement multitasking on a single core. So on one core,

on one hart, how do we make multiple tasks run at the same time? Well, what we actually adopt is this time-sharing approach, that is, letting these two tasks take turns executing, letting the two tasks take turns executing, taking turns using this hart to achieve it.

So each one, we know that once a hart is running, during its execution, its execution state of the hart is really reflected basically in the changes of those 32 registers. So if we want two tasks to be able to execute at the same time,

then we will inevitably need to, at the software level, For each of these tasks, the values of the registers it is currently using actually need to be backed up. For example, if we want to let the task on the left run, we need to take the register information

of the task on the right, back it up, and save it. Then, when we put the instruction stream on the right, onto the CPU, load it up and run it, we just need to take the information above and restore it back up there, and that's it.

So really, it's about doing this kind of switching, this kind of switch. Now, in the process of this switching, these instructions, these registers, This is actually what we call the concept of context. So if we say we want to run multiple tasks on one core,

say we have two tasks, then the context of both tasks needs to be maintained by us, and we keep switching which task is running on the CPU. We need to restore this task's context onto the CPU to run, while at the same time saving

the context of the other task away, and then do the switch. Through this context switching, we actually achieve the effect of multiple tasks multiplexing a single CPU, a single hart. This is actually equivalent to what we call

the concept of a thread. Of course, we call them tasks here. That's a more general term. So what I'm telling you here is this: the so-called task, actually corresponds to the current values of these registers. So when we maintain them,

when we save and restore them, we can actually, in memory, define a struct like this. We do have this struct in our code. You can look at our... this... Since today this slide corresponds to our 03 context switch in Shanghai. So starting from this slide,

you can see that we introduced a new file, called... it's right here, entry.s. Let me see where this struct is defined. It should be defined in... Let's take a look at where this struct is actually defined. It's not showing up when I search.

Hold on, let me check. Where is that file? Right here we're in this os.h file. We included this os.h header. I just didn't find it before. Right here we define the struct context. Now you can see that it actually has all the registers

defined in this struct. Of course, we didn't put x0 in here, because as we all know, x0, as we learned earlier, is actually a register whose value never changes. So we don't need to maintain it. As for the other values, we do need to maintain them.

Good. This is what's known as the concept of context. Now, with this concept in place, let's take a look at the so-called cooperative multitasking design and how to implement it. So here, we should mention one thing, which is that

when it comes to multitasking, we have in principle, we divide it into two types: one is called cooperative multitasking, and the other is called preemptive multitasking. What we ultimately want to implement is a Preemptive multitasking

But before that, before we do that, let's first take a look at how to implement a cooperative multitasking, how do we implement it? So, what is cooperative multitasking? In English, it's called Cooperative Multitasking. So what does it mean?

Well, imagine this: suppose we now have two tasks, the two tasks are like two people, and they have to share one HUD, one CPU. So what does cooperation mean? Well, say at the beginning, It is this person running on the CPU. Then after it runs for a while,

it needs to be able to actively yield the CPU. After yielding it, it essentially hands it over to the other one, and then it comes in and runs. After it runs for a while, it yields the CPU again, handing it over to the other one,

and then that one comes in and runs. So it is essentially two people actively giving this up, actively giving up the CPU, so that other tasks have a chance to execute. Now, this way of working is called cooperative multitasking.

Cooperative multitasking. In fact, we probably don't encounter it much these days, but in early operating systems, It really is a cooperative approach to implementing multitasking. For example, early systems like DOS, these older operating systems,

they actually worked this way. Now, with this approach, you naturally get the effect where two tasks take turns running on the CPU. But as you know, this approach also has a very obvious problem, namely, it requires that, when we write these tasks,

we know a task, as we mentioned earlier, is really just a function, right? Now these functions in the end, are still written by us. So if you need to actively give up the CPU, that means in this code, you have to call some special functions

to release the CPU, that is, to voluntarily give it up. But this thing, if the programmer is not careful when writing it, for example, if he just forgets to do this, and he ends up in an infinite loop, think about it, if his code is badly written

and gets stuck in an infinite loop, he never gets the chance to release the CPU, then the other tasks there's no way to execute so with this approach, it's very easy for our operating system in fact, it's very easy to be written badly by tasks

and get deadlocked by them so this cooperative multitasking does have certain problems then later we evolved into, that is, using preemptive multitasking now with preemptive multitasking, the idea is basically this: we don't want to let the current task

do this by itself instead, the operating system decides the scheduling of tasks it can our operating system can actively take away the current task's use of the processor and then give the processor to other tasks In this case,

you can imagine, that is, even if your task falls into an infinite loop, it actually won't affect the entire system. Now, as for how to preempt it, we'll cover that later on. For now, let's first take a look at, that is, how simple

cooperative multitasking is implemented. In this chapter, what we're implementing is cooperative multitasking. Right. So we'll explain it to you with a diagram, that is, this one here. One way to implement cooperative multitasking,

that is, of course, there are many ways to implement it. Here, we'll take the approach used in our code and walk you through it. All right, so, over here, you can see there are two tasks. One is task A, and the other is task B. Right?

Now, with these task A and task B, as we know, a task is really just a function. And a function, whether or not it calls other functions inside, as you know, you can think of it as simply a sequence of instructions. It's just a sequence of instructions.

So look at task A here. The instructions go from for example, I, I plus 1, and so on, all the way to I plus M. That's one sequence of instructions. Then for task B, it starts from J, J plus 1, all the way to J plus N. That's also a sequence of instructions,

right? So this is the first concept. Now the second one, since we said this is cooperative multitasking, that means we have to, within the task, insert some instructions ourselves, voluntarily giving up the CPU. So here, in the red part,

So this call to switch2, is essentially this effect. Of course, the process of voluntarily giving up we summarize it as one function, switch2. So that's equivalent to saying this function, this task A, as soon as it calls switch2,

we can imagine that it yields the CPU. Then at that point, we can take another task and load it in. So what exactly does switch2 do in there? Actually, based on our earlier introduction, we can know that all it really does in there is one thing:

That is to say, since it's going to do a switch, then it needs to not only switch this and load this task in, or take it out, there's actually one more main job it has to do, which is to save and restore this context. So you can see what it does,

the first step is to save the context of the previous task, and then, at this point, it performs a switch, switching the context, which means switching the task. Then, when it switches, it still has to do one more thing afterwards,

which is to take the new task's context and load it in. So actually, its main job boils down to these few steps. Now over here, look at the return of this function, the last return. Once this return is executed, we can achieve it so that

a new task down below starts running. As for how that process works, how is it actually implemented? Let's look at the next diagram. So we know that right now these two tasks, assuming in this model of ours, there is only one CPU for the task,

just one CPU. Now inside this CPU, we know that this CPU, has quite a few registers in it, right? There are general-purpose registers, and there are also some CSRs. that is, the control status register. Now, in here, there are two registers we mainly care about.

The first register, is this register, ra. Because this thing is related to our ret and call instructions. Think back to when we talked about assembly instructions earlier, right here. All of these concepts will be used here. ra, as we know, is used to store the return address.

So, there is also one more register, called mscratch, a register. This register, it is actually a register under privilege mode, a privileged register under Machine mode. We use this register, What is it used for? We now use it like a

as a pointer, used to point to our which context is currently being handled So for this purpose, we actually need to give every task its own allocated context space. So these two things, you can think of context A and context B,

as actually being located in two blocks of space in memory. Inside here, what is stored is this task's own general-purpose register contents. This actually corresponds to what we just introduced to you, which is really just a struct block like this,

the context struct, which we define in memory. one for each task. And then we use this, which basically means we'll use this mscratch register, sort of like a pointer, pointing to it at one moment, and then to the other one at the next.

That's how we implement the context switch. Switching context is basically just doing this. Let's take a look at how this process runs. Let me walk you through it. First, of course, the first step is to do some initialization.

Now, the initialization work, we essentially need to initialize some of the contents in these registers. Initialize them. So at the very beginning, since we have two RAs, the addresses, we initialize both of them to the address of the first instruction of our task.

On the left is i. On the right is j. This is the initialization. After the initialization is done, we then initialize this mscratch register as well. Its initialization, at the very beginning, let's assume the first one, the task loaded is task a,

then we make this register, point to this, the context on the left. The left points to this, the address of this context on the left. It holds an address inside. All right, that's the initialization. After the initialization is done,

we start running. So take a look, everyone. Suppose at the very beginning, it starts running from this task on the left so what does this represent this represents the pc value we talked about this earlier, right it's our program's

current execution address this pc pointer the value in the pc register so first we run this task on the left first we execute the first i instruction then we execute the second i+1 instruction now the key is when it executes this instruction

for example, this task on the left wants to voluntarily give up the CPU so it has to call this switch to function now everyone knows that this call function not call this function, but the call instruction this call instruction

in assembly, what does it actually do it actually puts As we mentioned earlier, it will take the next instruction after this instruction, which is the address of this instruction, and put it where? It will put it into the CPU's RA register,

right? We covered this earlier, right? So when we execute this call instruction, it will take the address of the RA + M instruction, and save it into RA. This is all just the normal function call process, right? And then, it will jump into the switch2 function to execute.

Now, this switch2 function, for switching this upper context, is the core logic, its first step, is to save the previous one. The previous one, as we know, now what we mean by the previous one is that one. The previous, the previous context

it becomes next instead because we are about to switch to it So when we say the previous context we actually mean it Now, its context, what we mean by context, is really just what we call saving the idea of saving the context, basically means that

we take the values of these registers inside the CPU and move them out of the registers into the memory area corresponding to this task into that memory into that context area and save them there and save them there So as you can see,

this value, originally this was RA plus M So in the following process, after saving it, then for the one on the left, go into its stack frame, and this ra will become what? ra plus m. Right? So once this becomes ra plus m, this is saved,

and of course there are some other registers as well, which we won't go into. They all need to be saved. Once the saving is done, we can proceed to the next step, which is the switch. So what we mean by the switch, as I just mentioned,

is really just switching this pointer. It used to point to it, and now we switch it, pointing it to the task on the right. So the so-called switch is really just taking this pointer and switching it from the left side to the right side.

Then, after switching to the right side, as we know, this task now on the right is going to start doing what? It's getting ready to run on the CPU. So in order for it to run, we need to take the things inside it, the contents of its original context,

and restore them into our CPU. Restore them into the CPU to perform the context switch. So the next step, this restore of the next context, is really about taking the values inside the context of task b and restoring them into the registers,

loading them from memory into this loaded into this CPU's registers. Now, the instruction it uses, think back for a moment, it should actually be what? That is, for the one earlier, the one writing to memory, what was used? It should be the store instruction,

right? And over here, it should be the load instruction. I won't go into this, you can go look at it yourself. So this restore process will take this value j and load it in. So here it becomes j. Right? Now the next step is the key return statement.

We know that this return, this return instruction What it will actually do is it will go by this according to the value of this ra and jump to the corresponding place Now, here is a j Do you all know what this j means? In fact, it is actually the position of this instruction

Right, everyone? So, this thing after the e-return our PC pointer will naturally jump right over to taskB And that is how a jump between tasks is achieved In essence, it is still a function call In essence, it is still a function call

But this function call is different from an ordinary function call Because with an ordinary function call the context does not change But in the current situation Besides achieving the function jump, what's crucial is that we switched the context

So the whole execution environment changed The entire running environment became a new task On this CPU, running in a brand-new environment. And this is how we achieved a task switch. So what comes next, none of this is complicated — it just continues executing.

Now moving on, let's demonstrate switching back. Suppose TaskB, also doesn't want to run anymore, and it wants to release this CPU. Well, this process, based on our earlier analysis, is actually quite similar. So the first step,

it takes the address of the J加N instruction just below, and stores it and it's saved in here. Right? It gets saved in here. By a store instruction, that's the effect we get. And then, we say that it jumps right here. Then likewise, saving the context,

and then, switching. Right? Switching. After the switch is done, it restores again. That is, it takes task A, and restores it back in. Task A, the contents saved in here, will all be restored back in. Restored back in. After the restore is complete,

in the same way, it must return to where? To this one. This — at this point, once the restore is done, the i plus m we saved earlier takes effect. Because at this point, the return still looks at this value. So at this point it jumps to

the instruction i plus m. And, it happens to be exactly the next instruction after our call instruction. So this task is essentially — if you don't count the process of being interrupted in the middle — you can imagine this this task

is actually just running in — running continuously Now of course, this switching process is very fast. If the switch is fast enough, you basically can't see the switching that happens in between, and so you can So from the perspective of a single task,

it is actually still running smoothly, and that's how we achieve task switching. That is the whole switching process for us. The core content is really right here. to perform the tasks. Now that we understand this, it's really just a matter of looking at how our code implements it.

So this key switching function, corresponding to what we just had, is very simple. Our switching function, you can imagine, this switching function, we wrote it in assembly. Because this switching function has extremely high performance requirements.

During the switching process, it will be used very frequently, so on our side, it is written in assembly. Now, building on what we learned earlier, we talked about before how, in practice, assembly can essentially be defined together with C code.

So when we write it this way, you can actually imagine that there is a C-language function like this. Let me sketch it out as a demonstration. Its first... it has only one parameter, and this parameter points to the address of the next context.

So this address, according to our calling convention, as you know, is actually placed in the register a0. Then when we come in, here, this and this thing, are actually two assembly macros. Take a look at them after class. What they achieve is taking this context,

this one is save. So what does Save mean? storing the registers in the CPU into memory. And then the so-called restore is taking the context in memory and putting it back into the CPU. So those are the two macros. And in between is a switch.

This switch, as you can see, actually makes use of The CSR instruction we learned earlier It is actually This CSR instruction, everyone remember it? We talked about it before Combining with what we discussed earlier It is actually just a write operation on the CSR register

A write operation on this register A write operation Write the value in A0 into it Now we know that A0 is actually a pointer A pointer to the next context So once you write it, doesn't that achieve the effect of changing it? Right?

Look at the diagram from earlier and think about it yourselves Then we won't say more about these earlier parts This is mainly because in this process It uses T6 as a parameter I won't say more about that Then this CSRRW Actually, this achieves the two registers

you can do the swap in a single atomic operation. Now, as for why we use t6, we have a comment about it in the code. The reason is actually that during this execution process, it needs to save every general-purpose register. And this t6,

it is actually in our set of general-purpose registers, it should be the last register. So we use it as the parameter of this function, because it is the last one to be overwritten. Nothing else to it, really. This function is very simple,

but you need to understand the underlying principle. So our overall, this whole Shanghai document that is, cooperative multitasking, we actually complete it in two steps. Let me briefly walk you through it. In fact, in our code,

um, Three and four, these two things are both about task switching and cooperative uh multitasking. Now, four, in the end, actually implements cooperative multitasking. As for three, you can actually think of it as, before we uh implement the final cooperative multitasking,

we need to try to figure out how to implement the first step. This so-called first step is the context switch. How do we implement this step? So this example three, only implements the context switch, this first-step example.

Its design is very simple. That is, we first define a context, and then, we now define the first task. task0, this first task, this task doesn't do much. It just keeps printing, about every 1000 ticks, and then it prints "running".

This task will never exit, it keeps running in there forever. And then, correspondingly, for this task, since it needs to have its own stack, we need to define a stack of its own, and define its own context, since we only have one task here,

we just define one corresponding stack and corresponding context, and that's it. And once we have that, we implement an initialization. Note carefully here, this corresponds to the initialization process that I explained to you earlier.

We're actually taking this task's Here we're actually initializing this stack. This is actually what we explained to you earlier. We take the first instruction of this task's The function's name is actually the address of that function.

In other words, it's the address of the function's first instruction. Take the address of that first instruction and save it into ra. If you combine this with the initialization part of the animation we showed earlier, you can see what's going on here.

Then, the first step to making the switch work, that is, switching to the first user program, so what's here is actually still the same task as before. The thing we actually need to implement is this function called schedule.

For this schedule function, internally it just calls a switch2. But this function, note that this function is actually defined in assembly. And as for the argument it passes, That is next. next is this one. This context is actually it.

It is our first task. The corresponding context. We take the context of this first task and pass it into here. Once this function is called, it switches into the first task. And then, our after the kernel is up and running, as everyone knows, it actually goes through

the start_kernel entry point. Inside it, the first step is to do an initialization, and then this step performs the task switch. So please note, what we are really emphasizing here is that this task, in fact, even though we call it task zero,

Actually, from the perspective of our whole system, it can really be considered the second task. Why do I say that? Well, the setup process we did earlier, you can consider that as our task zero. This switch, switching over to this task,

that's actually our second one, so you can think of it as task one. Please keep that in mind. There are really two tasks. It's just that the task we initialized, it runs very quickly. Once it gets to this step and sets everything up,

it jumps over to the first task. That's our first step. Now, the corresponding part here, as for the code, take a look at it yourselves when you get back. This one. I can't switch over in time. That is to say, what we're talking about now, this thing in 03,

when we run it, you can see that at the very beginning, actually, this printing of hello rbos, you can think of it as task zero, the very first task of our system, it is running, and while it's running, once you see this thing printed out,

that means we've actually switched over to our first user task, our first task has started running. That's actually what it means. So when you see this thing printed out, it means our first task has already switched over, which means the context switch was successful.

Then our fourth example, that's our cooperative task, that is, Multitask, that's the one that implements cooperative multitasking. Let's take a look at the effect first. As you can see, look at this, it keeps changing, right? Now, during this process,

we actually have two tasks running. 0 and 1 are running alternately. This alternation process is actually achieved through this cooperative voluntarily giving up the CPU. These two tasks, let's take a look. Now, in this implementation of

this multitasking scheduling, when we implemented it, we actually made some improvements on the original basis. You can compare them. Take the 03 and 04 examples. The main modifications are Let me demonstrate this for you. It's right here at this point.

So originally in 03, it only defined one task and one task stack. But after we moved to 04, we actually made a few improvements. What we actually did was define, we took all the task stacks and the task structs, and defined them as an array.

Right now, in other words, in our system, we can support up to 10 tasks, 10 tasks running at the same time. 10 tasks. And then, when we use it, we actually implemented that is to say, the task selection, we do it through this variable.

top is used to indicate how many tasks we have created in total. Then current is used to point to the current task. Since it's an array, we just define an integer, and its value is actually the index into this array. So this integer value

is used to point to the current task. Then here, every time schedule is called, we actually use this logic to compute the position of the next task. Now this, as you can probably tell, by doing this modulo operation, you can actually achieve a round-robin

a round-robin. So what we use is a round-robin approach. That's why, if you look at the output, it first goes from 0 to 1, and after 1 finishes, it goes back to 0. Because if we say we only have two, then this top value is actually 2.

And with 2, they just cycle between 0 and 1. So taking the modulus gives us exactly that effect. Everything else stays the same. And then we took this schedule, wrapped it up a bit, and turned it into a task_yield. Yield means voluntarily giving up the CPU.

So for our two tasks, we created them as two tasks. Then we also defined a TaskCreator, used to create the tasks. Inside this main function, we can create two tasks, pass in the functions for the two tasks, and we get two tasks created.

And then inside each task, we actively call TaskYield. to give up the CPU and then implement the switching between two tasks to achieve that effect This code is actually quite simple So here I just didn't go into that much detail

You can mainly I'll leave it to you to read through it on your own So regarding this cooperative multitasking, that should be about it This is the calling situation within the group functions The calling situation within the group functions

Inside the group functions, basically what happens is first initialize schedule initialize the related stuff and then OSM Inside OSM, as you can see It's through task_create that we created two tasks. And then, when schedule is called again,

it actually switches from our task 0 here, that is, this one, switching over to task 0. First it switches to task 0, and then the two tasks begin, switching back and forth between each other. That's the effect schedule achieves.

Now, once the two tasks start switching, right here, look, why does he say we never come back here again? It's because it never switches back here. That is, the task represented by this start_kernel function, this task In fact, it won't switch back

Pay attention to this Actually, there's no way you'll see this line printed You'll never see this line So this is a cooperative multitasking simple implementation After you go back, based on what we covered in class and the explanation I just gave you

take 9, 9-1, and 9-2 and give them a try Now, regarding this cooperative multitasking, the most essential part is really that you need to understand our concepts about the related initialization and, during the task execution,

how this switching process is actually implemented Once you understand this diagram, when you go and look at the code, So I hope this code will be a bit easier for you to understand. So for today, that's really all the content I wanted to cover.

With the remaining time, this code really still needs you to spend some time digesting and understanding it. So for the rest of the time, we'll leave it to you to go through today's material, including printing to the console window,

memory allocation, dynamic memory management, and also the code for cooperative multitasking. Go through it on your own, and once you understand it, do the corresponding exercises, and that should be about it.
