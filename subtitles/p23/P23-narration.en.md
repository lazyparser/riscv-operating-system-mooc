# English Narration Script

Next, we're going to cover Chapter 13. In Chapter 13, what we want to introduce is how to implement preemptive multitasking.

It breaks down into a few parts.

First, let's look at what preemptive multitasking actually means. Earlier in the course, we showed you how to implement cooperative multitasking, right? Now, cooperative multitasking is relatively simple. At the time, we actually used function calls and returns,

and that's essentially how we implemented a cooperative scheme. Cooperation is simple to implement, but its drawbacks are pretty obvious. It requires each of the tasks to actively call the task_yield function to give up its use of the processor

and hand it over to another task to run. Now, in a lot of real operating systems out there, this kind of cooperative multitasking has actually been gradually phased out. Instead, we mostly use what's called through preemptive multitasking

That is, we want the operating system to decide by itself which task gets to use the CPU, so it can proactively take the CPU away from a task in order to reschedule, rather than having the task give it up on its own. So how is this actually implemented?

In fact, it uses some of the concepts related to interrupts that we introduced to you earlier. So next, let's take a look at how this preemptive multitasking is implemented. Now, let's first look at this diagram. In this diagram,

we can see there are two tasks, right? Task A and Task B. Now, this Task A, its instructions, starting from I all the way to I plus M, keep executing one after another. And B, starting from J to J plus N, starts executing. Now, since what we're implementing here is

preemptive multitasking, as you can see, with these two tasks, inside them, they don't call any task function to voluntarily give up the CPU. So once any task is running, how can we let another task also get to use this CPU? Right?

That's exactly what we want to achieve. Well, the method we actually adopt, is to make use of what we learned earlier, This interrupt approach is actually what we used earlier with the timer interrupt. We make this interrupt occur periodically,

and then each time the timer expires, once the interrupt fires, we can handle it in the timer interrupt handling process, where the kernel — that is, our operating system — schedules these tasks. That is essentially the effect we want to achieve.

Now, once this interrupt occurs, based on what we have learned, we know it is actually a trap, and in the trap handler, we will perform operations like these. These operations are actually very similar to the cooperative approach from earlier,

though of course the implementation is somewhat different. because after all, it is implemented in the middle of an interrupt handling process but the overall idea is still similar to what we saw before the first step is to save the original context

then switch the context then restore the next new context and finally since this is an interrupt handler of course it ends up calling mret to return that is something you should be aware of next, let's take a look at this inside the chip handler function

it actually what its execution process looks like in detail how the details are implemented based on what we just introduced we know that after chip occurs we actually make use of the timer system interrupt and in fact, inside this chip function

you can imagine that we will definitely check Is this interrupt a timer interrupt, or some other exception? Now, if it's some other interrupt or exception, then its handling flow is really no different from the exception handling

and external interrupt handling that we introduced earlier. So the logic on the left is perfectly fine, meaning it takes the path on the left, which is the same as the original logic, with no difference at all. Now, if it's a timer interrupt,

then we need to do some extra work on our side. Apart from what we just mentioned— maintaining an internal clock tick— the core operation on our side is that we will call the original switch2 function in here, to perform the context switch.

And then, finally, it actually still exits the interrupt via mret. So the logic is actually still very clear. Now, as for the concrete implementation, we need to take a look at it. That is, how do we implement it? So when an interrupt occurs,

according to our earlier design, we know it will actually still go through this logic. It will definitely still go to the trap vector, into this entry point, right? And inside here, it saves the context, and so on. As for the specifics,

we'll talk about that later. That is, there are also some changes in here, but the most essential part definitely still removed that chip handler processing function, right? Now, as for this function, we know we now, based on our earlier introduction,

when it comes in as an interrupt, it will actually go into the timer handling logic, that is, this function, it will go into this one here. Now, the logic of this handler, we have some changes to it now. Besides some things, like maintaining a clock tick count,

the most core part is actually that it will call that schedule function. Now, as for this schedule function, Actually, inside it, in the end, we call switch2, this function, and this function, is actually implemented in our assembly code.

Now, this switch_to function, eventually still goes to mret. This corresponds to that flowchart we had earlier. Let's take another look at it ourselves. That is to say, so this entire trap handler, the way it's described here might be slightly confusing.

So this trap handler, don't confuse it with this trap handler, this actually represents, I mean, this entire processing procedure, this whole processing procedure of ours, the entire processing procedure. So this trap handler,

this whole processing procedure. In fact, it is after making such an active call, and finally going through mreturn, we return to our normal processing flow. But in this process, because at its core, it actually implements a switch2 process,

and in there, we achieve our task switching effect. So this is our trap handler function's implementation process, which I've walked you through. Now, in terms of the design of these related data structures, we also need to make some corresponding adjustments.

Because, why is that? Because, if we recall, in the case of cooperative multitasking, our that is, not the trap handler, that is, inside this function called switch_to, it actually goes through not through mret, everyone still remembers, right?

we used that the function's return instruction to achieve it. Now, as we know with the return instruction, why can we return back? Actually, it's because we make use of this register, ra, right? Indeed, the register ra, but here,

since it uses mret, we know we need to to return from here Actually, which register should we be using? We are actually using the mepc register right? to implement the return, returning from an exception, returning from a trap. So back when we were in the coroutine,

this ra register of ours, in our context, of course we had already saved it. But right now, since we want to use the mret instruction to return, then we cannot use ra anymore. We actually need to take this register, and also save a copy of it

in our context. So when we implement it, at this point, we actually no longer care about that ra thing. Because in this trap handler, we actually don't care much about the ra register, what we care about is the mepc register. I wrote that wrong earlier.

It should be mepc. This spot here. What I wrote here earlier should be mepc. This register here. Now, since this register, takes part in our task context switching, so naturally, for each task, when we maintain its context, we also need to maintain something equivalent to epc.

Of course, we named it pc on our side. Of course, if we called it epc, it might be easier to understand. Actually, both this pc and epc are used to maintain the contents of that register. We'll take it when a task switch happens

perform the corresponding save and restore, and that is how we implement task switching. The other parts are actually very similar. Now, specifically, how does MEPC work in the quantum task? How does it achieve the effect we want?

Let me give you a simple demonstration. So next, let's take some time to do a simple demonstration for you. So first of all, here is some of the relevant code. As we can see in this code, our context struct here, has been adjusted a bit.

Besides, in the coroutine room from before, we still save those registers in the same way, but over here we have also added and defined a new variable called PC. This variable is actually used to, to be precise, it should be used to save that EPC,

used to save the EPC. This part may not be written very accurately here, but I want to emphasize this point: it saves the EPC. And then, in this function, we also made some corresponding adjustments. So after the trap vector comes in,

in the previous logic, we would only save all the general-purpose registers. But now, we added some more logic. As you can see, the logic we added, the logic here, actually saves MEPC as well. It saves it into the context. Please take note of this.

And then, according to our logic, here it just goes and calls that handler function inside the handler function, as we described earlier, it will actually trigger another call to the switch2 function and this switch2 function will ultimately call mreturn to return

it will, in the end, ultimately call mreturn to return and before calling mreturn to return, it will save all the general-purpose registers as well this corresponds to what we saw earlier these two correspond to each other and similarly, corresponding to that,

we have some logic on this side too which is to restore the EPC as well so here, please pay attention to this, when you're reading the code, it's these two parts that we handle When we grabbed the temporary multitasking here, we made some corresponding adjustments.

And let's pay attention to one thing, which is the switch_to function. Because the whole flow we have now, it actually goes around in a circle. We actually come in from the trap vector, and then go out from here. So for this switch_to function,

the saving logic that used to be there, it's actually placed over here in the switch_to function. Right? If you still remember how it was implemented during cooperative multitasking, then now, with this logic, we basically removed it from switch_to,

and instead we reused this logic from the trap vector. So on this side, there's no save logic anymore. There's no save logic on this side anymore. Please keep that in mind. These are all some adjustments for this preemptive multitasking.

Now, after understanding the related code and struct adjustments we just made, let's take a look. In a preemptive multitasking system, how does this process actually happen? Let me give you a simple animated demonstration. All right.

This diagram, picking up from what we just looked at, that is, on the left is one task, and on the right is another task, right? And this one, on this side, is its context. And on the right side, is also its context. They are in memory.

Good. Now this blue-colored part, it actually represents the CPU so let's assume there is only one CPU then how do we implement these two tasks switching on this CPU let's take a look together so first is initialization just like the previous one

that is, we first take the address of Task A's first instruction and put it in here do some initialization the right side is the same put ra in there right? nothing special about this then, down below we need to use mscratch that is, initialize first

assuming this Task A on the left is the first task we want to schedule So, first, let's take First, let's point the mscratch register to the task on the left, to its corresponding context. This way, it means we want to Right from the start,

we run the task on the left first. Good. Then the PC starts running. So let's assume it first executes the first instruction, and then goes on to execute the second instruction. Now, when executing the second instruction, the value of PC down here

this value here, is also changing accordingly. This value is changing as well. Good. Now let's assume at this point, this timer, For example, when we reach the timer, once the timer fires, we know that the trap handling flow that we learned earlier

will kick in. Now, as for the trap, we know that in its handling flow, one very important point is that it will take this — since this was an interrupt, right, it was an interrupt — it takes the address of this instruction, the I plus 2 instruction,

the address of it, and saves it into mepc. So you can see that at this point, it will become an I plus 2. What about I plus 2? Actually, it is this instruction's the address of this instruction And saving it here, we will see later on,

it will serve this task after a switch and then a return later, it will come into play. Let's take a look at how this process happens. So suppose its MEPC now, has become I plus 2. So it becomes I plus 2, and then, this trap, at the same time, it will jump to this

the entry of our trap handler, to perform the corresponding operations. So the first step, according to the code we had earlier, the implementation of it, its first step is to take the previous context's So the so-called previous one is what's currently pointed to

the contents of this context need to be saved By so-called saving, we mean taking some of the contents on the CPU and saving them into our memory Now, in this process, since we're currently focusing on mepc, so it actually we won't talk about the other registers

So actually it will take what's in mepc, i+2, and save it in there on the left So on the left side here, it will also become i+2 right? Now after saving is done, the next thing we need to do is switch the context Now by so-called switching,

So basically, what we do is take the pointer stored in mscratch, which originally pointed to the left side, and now make it point to the right side instead. Then, next, we need to get ready to schedule the task on the right onto the CPU

and start running it. Now, before it runs, what's the first thing we need to do? We need to take the context on the right and restore it onto the CPU, because what we're doing now is replacing the entire execution environment currently on the CPU,

essentially swapping it out, replacing it with the task on the right. In this swapping process, once again, we only care about MEPC. Looking at MEPC, what value will it be replaced with? it gets replaced with this the original initialized return address,

which is the first instruction of the task on the right, because the task on the right is about to start running, right? Now the i + 2 value originally in there gets replaced. But don't worry. Why? Because even though this value gets replaced,

we've already saved a copy of it in the original location for it. Pay attention here. In fact, this one saved here, so this replacement over here won't lose anything. Pay attention to this. So this one just gets replaced. It becomes connected.

Now, then, think about it — after this is done, when we start executing mret, the process of switching our task will happen. This will happen. Because, as we know, with mret, it actually takes the value in mepc — it jumps based on the value inside there.

So if this is set at this point, then it will actually jump to that location to execute. So once this mret executes, our pc actually jumps to task b, and it starts running from its first instruction. At this point, we have completed the first switch of this task.

Next, let's look at how the task switches back. Switching back is actually a process that, just like before, It's actually quite similar. So let's assume this PZ, starts running again, continuing downward, proceeding to J+1. Now, since this task,

did not give up the CPU voluntarily, we don't know when it will switch. But the actual switch, still happens at, well, the moment of this interrupt. At this point, an interrupt should occur. We haven't drawn this interrupt yet,

but you can imagine it. Suppose at this moment, this interrupt happens again. Once the interrupt occurs, as you know, the same process we just described repeats. What is this repeating process? Well, that is, suppose the interrupt occurs at this point.

Once an interrupt occurs, it will take this instruction at pc plus 2 and save it into this field here. That's what I'm trying to say. And then, this side here becomes pc plus 2. So after it's saved, as we know, it jumps to this task handler,

into this trap handler function. Then it repeats the logic from before: save, save the original context, and then switch, switch. After the switch is done, restore. When it restores, as you all know, at this point This register i+2, this value,

it will be replaced by the value over here. But that's fine, we've already saved a copy here. So even though it gets replaced here, that doesn't matter. Same as before. Now, when we restore, this i+2 comes back in. So as you know,

at this point, moving on down, when we execute mret, where do we return to? Well, quite naturally, it goes by the value of this i+2, to jump there and continue executing. Right? So next, it comes right back. And after that, there's nothing more to say.

So what we're emphasizing here is really this point: in this process, even though our task didn't voluntarily give up the CPU, with the occurrence of this timer interrupt, with all these things happening, in the middle of that,

we use the interrupt handler to carry out the scheduling and execution of the entire task. So that's essentially what we call preemption. That's exactly what it means. So in other words, the task didn't voluntarily give up the CPU,

but our kernel, preempted it. This is what's called preemptive multitasking. So up to this point, we've finished introducing preemptive multitasking to all of you. Now, some of you may ask, I mean, preemptive multitasking once we implement it,

then the original, this backward-compatible kind of if the original cooperative multitasking, actually refers to I mean, our the Task_yield interface, does it become useless for us? Because it's also possible, as you can imagine,

it's possible that during the execution of our program, we might a task might want to voluntarily give up the CPU, for example, we don't want to wait until the timer fires before we get switched out, because we may have finished our work,

and then we can release the CPU right away. So, this Task_yield function of ours, In fact, we still have a need for its existence. So how do we maintain compatibility with this? Well, we actually need to introduce that is, for our third type,

the introduction of the software interrupt, we actually implement it through this software interrupt mechanism. This allows us to achieve that is, on top of the existing preemptive multitasking, we can still be compatible with this kind of cooperative multitasking.

Now, this software interrupt, as we introduced earlier, it still uses the CLINT device, because this local interrupt, the local interrupt generator, it can generate software interrupts, and it can also generate timer interrupts.

So this thing, that is, why did we think of using a software interrupt to do this? Well, think about it. That is to say, if we use the timer interrupt to implement preemption, what effect does that actually amount to? It means that even if a task doesn't voluntarily give up the CPU,

then through this... after we set this thing up, it generates a timer interrupt, and it can trigger us periodically, like giving us a phone call, so to speak. Think of an analogy, everyone. It's like saying, we tell this thing,

our task is running up there, and before we start running it, we have already set it up so that this CLINT, this thing, as a peripheral device, for example, every one second, gives us a phone call, and then after it makes that call,

Our kernel, in the process of receiving the interrupt, switches this task actually to achieve this effect this is the concept of preemption. Now suppose we want to voluntarily give up ourselves, what does that actually mean? It means that since we still want to

reuse the original code, actually the main goal is still to reuse the original code. And the original code, as we know, actually comes in through the interrupt entry point, comes in through the interrupt entry point, and then implements this preemption.

So how do we make this task, during execution, for example, while I'm executing, And then we call a task yield So, we call a task yield, which can actively go Basically, it's like actively making a phone call to yourself, that is, actively triggering an interrupt.

Now, this is the concept of a software interrupt. Because all the other interrupts, in fact, are not triggered actively by us, right? A peripheral interrupt can happen at any time, and as for the timer interrupt, that's not actively triggered by us either,

and this timer interrupt, we don't trigger it actively either. We just set up a period ahead of time, and when that period is up, it generates on its own, generated by this external thing, and as for the software interrupt, you can understand it as meaning that,

basically, we go and set this thing up. Of course, we do it by issuing a command to set up this thing, and then it generates an interrupt for us. Now, this thing is actually just like an exception— relatively speaking, it's a bit like a synchronous process,

a bit like a synchronous process. This kind is called a software interrupt, that is, an interrupt triggered by software. That's what a software interrupt is. So once it's triggered by a software interrupt, you know, if it's like this—

once an interrupt is generated, we can just like with a timer interrupt, we can completely reuse the original handling flow and functions. Actually, the reason we introduce software interrupts, is precisely to achieve this effect.

So next, let's take a look at that is, how we make use of it. How do we trigger a software interrupt? How do we make use of a software interrupt? Now, since the software interrupt is still generated by the CLINT, this device,

so let's take a look at it first. Earlier, before this, in the previous chapter, we mainly introduced its timer part. Now it also has some registers that are related to software interrupts. Let's take a look at them together now.

It has a register called MSIP. This is, for each hart, there is one MSIP register. This MSIP register is rather special. It is also a 32-bit register, but this 32-bit register, in practice, we only use its lowest bit. The other high 32 bits

Actually, we don't use it. So what about the lowest bit? Pay attention here, everyone. In hardware, it actually gets mapped to this, mapped to the MIP register, to its special MSIP bit. You can think of it this way: when we write to this register,

once we write the lowest bit of this register, then this bit, because we know this bit is actually used to indicate that is, it is actually the so-called interrupt's software interrupt pending bit, the software interrupt pending bit,

which is used to tell us whether this interrupt has occurred. Now, since we're mapping to this, If we set the lowest bit of this MSIP register to 1, and we set it to 1, then this bit naturally becomes 1 as well, so through this bit,

we can also read out that the software interrupt has occurred. That is actually what this means. Don't let this confuse you. That is to say, this MSIP is the name of a register inside the CLINT, while this MSIP here, actually refers to

a certain bit of the MIP register, a certain bit position. Please pay attention to this, don't mix them up. This is the MSIP register. Now, using it, is very simple. As I just said, Let me emphasize this again here. That is to say, when we write 1 to this

MICB register — that is, this MSI control register — actually, writing 1 to its lowest bit will trigger this software interrupt. What if we write 0? It means we are acknowledging that interrupt. In effect, we are notifying the CLINT that

we have finished handling the software interrupt. This way, it will not keep triggering this software interrupt repeatedly. Otherwise, if you don't acknowledge it, this software interrupt will essentially keep being generated.

The signal level stays high, and then it will keep generating this software interrupt non-stop. So once you have finished handling it, you must acknowledge it. Writing 0 to it turns it off. Otherwise, this software interrupt will keep being triggered forever.

Please keep this in mind when writing to it. So based on the idea we just discussed, that is, by triggering a software interrupt, we go through the same flow to carry out the task switch. Now, based on this idea, for the function that this task used to have,

we rewrite it a bit. We can take the implementation of this function and, compared with the cooperative multitasking version's task yield, we can actually make a comparison. Here, it no longer calls that switch function, but in fact it

just does one very simple thing, which is writing a 1 into this mrcp register, writing this 1. But this action is very simple, yet this 1 We need to understand what actually happens behind the scenes. It actually triggers a software interrupt.

So once the software interrupt is triggered, it's really just the interrupt handling flow. So similarly, it goes into that trap vector function, right? And then that calls the trap handler. The trap handler in turn calls schedule.

So if we walk through the whole process, it's actually the same as the process of the timer interrupt we just saw that we generated earlier. Do you all see that? It's actually the same. This way, we basically keep the original logic unchanged.

The only thing is that I just took this function in task1 and and rewrote it, meaning that inside it just goes and sets this software interrupt register, and that's it. And then, please pay attention here, that is, at this point we must

clear it to zero. This way, after it's handled, we do a hardware return. This way, the software interrupt won't stay there forever. If you skip this step, You'll notice that the soft terminal will just keep coming in, again and again

Then the program falls into an infinite loop again which is not good So please keep that in mind So this way we have achieved compatibility with the cooperative that is, voluntarily giving up the CPU for this function So this function

we can still keep it still keep it At the same time, likewise we have also covered the interrupt part basically in its entirety That is to say, device interrupts timer interrupts and software interrupts we have touched on all of them

Through these examples, you can get an idea of how it works So for chapter 13, that's all the content we'll cover. Now, this example, let me give you a quick demonstration. That is, the final preemptive example, in the 08 preemptive example,

the preemption example, in this example, and in this example, we've also defined two tasks. Now, with these two tasks, pay attention here. Inside them, we only have, in the first task, a TaskHead. Let's comment this out for now. Suppose we don't introduce this thing,

and let's take a look. That is, suppose we, don't voluntarily give up the CPU at all, So this... So as you can see these two tasks, inside this apart from the delay, it's basically just an infinite loop, running on its own, it never actively gives up the CPU.

Now we again take these two tasks and register them into our system, creating them. Then we will also see this: since we have now replaced the entire underlying kernel with a preemptive one, these two tasks will still keep getting switched.

Let's look at the demo process. We can see — look, this 0101, it's switching again, right? This 0101 is switching again. And this switching frequency is of course driven by that timer. Whatever we set the timer to that's exactly what it is.

This task, one moment it's 0, the next moment it's 1. Of course, sometimes it prints two lines, sometimes it prints one. That's mainly because the timing isn't always that precise, so it doesn't necessarily land right on the printing point.

So, this, this is preemption. That is to say, in our code, we never voluntarily give up the CPU, yet they can still be switched between each other. So here, right here, Right here, let's add a voluntary yield. Let's take a look. That is to say,

after the voluntary yield, we can see that at this point the 'run' message hasn't been printed yet. Right after the create, it immediately gave up the CPU and ran it. So this 0 won't print 'running' right away. Let's see the effect.

Okay, let's take a look. See, as soon as task0 yields, we trigger a software interrupt. This is printed from inside the kernel. And then, you see this 1111, it just actively... Since it triggered a software interrupt, it will voluntarily give up the CPU.

So you can see that task1 starts running immediately. Over here, task1 starts running right away. Now this, and then afterwards, it's actually another context switch. Then the timer goes off. The timer goes off. After the timer goes off,

our task0 says I'm back. This I'm back, is actually reflected in this line here. Because at this point, this is a voluntary give-up, so actually, for this task, before the switch, it was actually stopped right here. And then, "I'm back", and it starts running again.

Right? And then, it just starts running again. And then, next, it's the timer-based switching again. Timer-based switching. Now, after this, it's just the two tasks switching back and forth on their own. This, I'll just demonstrate for you.

So through this example, I hope you get a sense of, that is to say, our preemptive approach— actually, the core of preemptive scheduling, is that it's done through interrupts. through timer interrupts to periodically trigger our kernel

to implement task scheduling Of course, what we have implemented the code we've implemented so far is relatively simple Now, based on this idea we can actually expand it expand it into a lot of much more complex task scheduling

For example, we haven't added yet task priorities right? And then there's also things like task sleeping, and so on All of these can be built on top of what we have now these most basic features. As for the more complex stuff, we'll leave that to you as an exercise.

and extend it ourselves. So, this about this strong multitasking, we'll wrap up our introduction here. As for the exercises, go home and work on them yourselves. That's all for today's content, just this much. We covered it quite fast,

and there was quite a lot of content. So for the remaining time, you can take this exercise, including the demo code from the course, and study it on your own. If you want to see how, during the interrupt and exception process,

those complicated registers I just talked about actually change, we can actually use GDB debugging, set breakpoints, and watch. Set the breakpoint at the task entry point. that is, breaking at our breaking at the entry points of these tasks,

and debugging it yourself. Break at the entry points of these tasks, and look at what happens before a task comes in and after it comes in, the actual changes in registers like mstatus, mepc, and so on. Combine that with what we demonstrated in class,

and watch it against the effect of that animation. That way, by slowing down the action and dissecting the whole process, you will gain a deeper understanding of it. All right, on the related content, that is all we will cover for now.

That's it for today. Let's take the code, and go over it yourselves first.
