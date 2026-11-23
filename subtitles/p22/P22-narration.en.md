# English Narration Script

Let's move on to the next chapter. We said interrupts fall into three categories: external interrupts, software interrupts, and timer interrupts. Timer interrupts are also very important, so we have a separate chapter to introduce them.

So, as for timer interrupts, I'll introduce them to you in these three parts. First, let's look at what a timer interrupt is. Now, as mentioned earlier, the timer interrupt is a type of interrupt. Right? And it belongs to a type called local interrupts.

That much is straightforward. So why do we call it a local interrupt? Basically, it's because it isn't triggered by a peripheral. It isn't triggered by those ordinary external devices. Rather, there is a dedicated component inside our chip,

a special device called the CLINT, a device built into the SoC chip, and it is this device that raises the interrupt. It's called the Core Local Interruptor, which is what the abbreviation stands for. CLINT is short for Core Local Interruptor.

So this thing, it comes with our chip specifically as a device attached to it. As for this device, it is responsible for generating two kinds of interrupts: one kind is called software interrupt, and the other is called timer interrupt.

We can see the blue line and the red line in the diagram. Now let's focus mainly on its timer interrupt. Now, for this device, we do the same thing. Similarly, in order to access it, you also need to access the set of registers it provides.

Now, this register, is also accessed through memory-mapped I/O. Its range starts at 0x200060, so there is a range like this. From this diagram, we can see which registers it provides. Let's take a look together. Of course, as we mentioned earlier,

Because the CLIC, this local interrupt device, can both generate software interrupts for us and provide timer interrupts. Today, in this chapter, we're mainly going to talk about the timer interrupt part. So the registers introduced here

are all related to timer interrupts. First, let's look at the one called Mtime. M still stands for the machine concept, and time, it's a so-called real-time, that is, a real-time counter. It's actually a clock driven by a crystal oscillator.

This clock, it runs at a fixed frequency, constantly ticking inside, it's actually a counter, it's a counter, right? It just increments at a fixed frequency. And this counter, it's what you'd call globally unique. That is to say,

suppose we have quite a few cores in here, there is still only this single, unified counter. So this counter, it is actually a counter that is 64 bits wide. The reason it's designed to be 64 bits, is that the range it can represent is very large.

I mean, suppose you're incrementing it at a very high frequency, as it counts up, for a very long time, it will never overflow. That's because its width is so large, 64 bits wide. And then, according to the convention in the specification,

which is to say, When power-on reset happens, the value of this register will automatically reset to zero. So it doesn't actually require any initialization from our software. After power-on, it automatically starts incrementing from zero,

at a fixed frequency. That's the first one, called the counter, the real-time counter. On some platforms it's also called ITC and so on, all similar things. We can go and read read this value. Now, as we said, don't we want to generate a timer?

Right? So how do we generate a timer? It only has that counter which is actually not enough. We also need something called a register called Time Compare. It is used to store a comparison value for the timer. How exactly do we use it?

We'll see that in a bit. First, let's understand what this thing actually is. This register, just like the other one, is also 64 bits. But when it powers up, it does not automatically reset to 0. So we need to set its initial value ourselves.

Now, below, this code, in fact, demonstrates that is to say, How do we set this value? We actually in the code, we actually defined a function called timer_load —a function like that, which takes an interval as a parameter. This interval,

its unit, is just a number. A number. This number, depending on, that is, the frequency, you set it yourself accordingly. As you can see, this value, it is per It is one per HART. Please note this. So, on this side, when we define this macro,

it takes a parameter, which is the HART ID. So, first, through this, we get a HART ID, for example, if there are multiple HARTs, then a value like 0, 1, 2, a value like that. And then, when it is passed into this macro, we can read out,

when we actually load it, we actually use this to, mtime That's the timer value we talked about earlier. Plus this. This interval. For example, say we want an interval of one second. If we want a one-second interval. Then this becomes a

corresponding one-second tick count. Then, after adding this value, we write it back and that initializes it. It loads a timer. During initialization, when they initialize, we call this function. And then, in the start function, that is to say,

when we come in, we call this init. So this is the call sequence. That's exactly how the process works. The calling process is just like this. So once this timer is set up, what is it used for? That brings us to what we're going to introduce next,

namely, this CLINT, this local interrupt device, and how it works. Basically, it will check, it will go and every time it increments the value of mtime by one, since this mtime value keeps incrementing at a fixed frequency, each time this value goes up by one,

it checks whether this value and the mtimecmp value we set earlier, have a greater-than-or-equal relationship. In other words, once this value is greater than or equal to it, it will generate an interrupt. It will trigger an interrupt.

In fact, we use this mechanism, to generate a timer interrupt. Now, one thing to note this timer interrupt is only generated once. So it requires us to, every time if you want to generate a periodic timer interrupt, you need to, after each time it is handled,

keep feeding this value, reloading it yourself again. Then, when the timer interrupt occurs, the hardware sets this MTIP, as we discussed earlier, this bit in the MIP register. When we write a new value into MtimeCmp, when we write a new value in there,

it will clear that bit again, marking that the timer has been dealt with. So what we have here is essentially some related examples in the code. One is the handler. Inside the handler, we need to reload the timer again. After it's loaded,

it will generate the next timer value. If we look at the logic of this timer loader, which we saw earlier, it actually takes the value of time, and adds an interval to it again. So after adding, this value is now larger than it again.

mtime is smaller than it again. Then mtime starts incrementing again. After it accumulates to a certain value, it will trigger another one. So, in this handler, if we load it again, it will actually produce a periodic timer. That's the effect we achieve.

Similarly, we need to there's one more thing to do, that is, in the handler processing, we need to handle the cost of 7 It's actually the timer interrupt under machine mode. We need to call this time handler. That's actually the trigger logic.

This is how it comes in. This part is all pretty simple. We just need to get a general idea of it. So overall, looking at the whole picture, if we want to implement a periodic timer, what is it that we need to do? I mean, logically speaking,

first we need to do an initialization. The initialization, the initialization, that is to say, this being set to zero, is actually handled by the hardware for us. We actually don't need to write it. And then, here, what we're actually doing is taking this compat value

First, add an interval value. So, and then, make sure to enable the interrupts at every privilege level. Then, this, as you can see, after this initialization is done, this, this device, it will run at a fixed frequency, constantly incrementing mtime,

adding 1, adding 1, adding 1, all the way until, when its value, becomes equal to the value of mcompare, once they are equal, it will trigger an interrupt. So at that point, the interrupt is generated. And then, after this timer interrupt arrives,

Of course, what you want to do is up to you. Then, after the handling is done, if you still want to generate it, if you want this timer interrupt to keep firing, then you have to manually update the mtime value yourself. So what this means is that,

if you add an interval again, then this thing will repeat and keep happening. This is how a periodic timer interrupt is generated. That's the logic of it. This logic is also very simple. Now, in an operating system, we generally

make a lot of use of this hardware timer, with many applications for it. The most basic application Let's say, this is basically a We know that the time management in our operating system is actually done using this kind of hardware timer.

So let me briefly introduce to you that within this operating system, we deal with some concepts, for example, what is called a clock tick, the concept of a tick, the concept of a TIC. Now, this TIC is a concept often mentioned in our operating system.

This TIC is actually a unit of time period. And this unit of time period is generally determined by what we just introduced to you, the period of the hardware timer. We can configure this ourselves. And in general, it can be somewhere between 1 and 100 milliseconds.

If it's 100 milliseconds, that means it happens 10 times per second. If it's 1, then it's once every 1 millisecond, which means in 1 second there will be 1000 of them, 1000 of these ticks generated. Now, as we know, with this tick value,

the smaller you configure it, the higher the system precision. But as we know, why does the overhead get larger? The point is, the smaller this tick value is, the more interrupts per second you generate. And every time there's an interrupt,

as everyone knows, whether it's hardware or software, both have to take part in handling the interrupt. So our normal flow gets interrupted, which causes some extra overhead. So when it comes to configuring this tick value, We need to configure it based on the actual scenario.

That is to say, we can't let the system get too busy, but at the same time we need to ensure it has a certain level of accuracy, and that's enough. So generally, it might be 10 milliseconds, or 100 milliseconds, which is usually sufficient.

Then, inside the operating system, we usually maintain a current tick value. At the beginning, this tick value might be set to zero first, or it could be that, if you possibly want to display this time as well, then you might have to

at the very beginning, read the current time we have, and then this tick count increases at a certain frequency After the accumulation happens, we can maintain a clock internally. That works. So this clock of ours, it's actually implemented through this hardware timer,

plus some extra processing. For example, on our PC, on a computer, if how do we get the time? Normally, we can have a peripheral on the PC, powered by something like a battery, and it can let this, on our system, just, even after the power is cut off,

it can maintain a, remember the current time. Then every time the system comes up, we go and read this time, and then take this time and Then, as this hardware timer starts running, we take this time and the time we read, accumulate them together,

and then record the current latest time. Of course, in the example we demonstrate to you, we didn't actually make it that complicated. We actually just simply let everyone see that we can maintain a tick count internally, and every time the hardware timer fires,

we increment this tick count, and then print out this tick count. So this is one of the most basic operations in our system timer. As for more complex operations, we may use them later as demonstrations, but we won't demonstrate them here.

So you can, in the exercises, Go ahead and extend these features on your own. But we know that in terms of the underlying principle here, at the hardware level it's really because it's implemented through this timer interrupt.

On our side here, let me just give you a quick demonstration of chapter 12, our corresponding example here, just a quick demonstration. So the corresponding example is 07, which is the hardware timer. Hardware timer. Now, with this hardware timer,

you can see that once it's up and running, at the very beginning, the tick count is 0, and then So every time it generates an interrupt, here we every time an interrupt is generated, after the interrupt, once we increment the tick count,

we print it out. So this, this example is very simple. Basically, through this example, you just get a feel for how we control the CLINT device, and make it generate interrupts periodically, and that's it. So this is our example on the hardware timer.
