# English Narration Script

Chapter 11, external device interrupts. It covers another kind of trap.

For the interrupt part, we mainly introduce interrupt classification, the registers designed for interrupts, the interrupt handling process, and also the PLIC, which is an interrupt-related peripheral. So there is one more thing:

once we finish covering the basic concepts of interrupts for you, we will show you something, that is, remember our UART device? In the beginning we used UART to print on the screen Hello RVOS, right? Well, at that time, we used what is called the polling approach.

Now that we've studied interrupts, let's take a look How to use the interrupt-driven approach to handle device input from the serial port That roughly covers the contents So here are the reference materials Let's start with the first part

The classification of interrupts So this When we talk about interrupts, traps are divided into exceptions and interrupts And interrupts are further divided Interrupts are further divided Overall, we can classify interrupts into

two major categories One is called local interrupts and the other is called global interrupts In fact, these local interrupts are further divided into software interrupts and timer interrupts So in fact, this corresponds to in fact, just interrupts

In fact, you can also think of it as three categories that is, software interrupts software interrupts timer interrupts and external interrupts. Now, this thing, in fact, in the RISC-V specification, is actually written out pretty clearly.

You can actually it is still this table. In this table, you can see that this table at the time, we actually just took the M-Course that table, Table 3.6, and removed the exception part from it. So we only look at the interrupt part.

Then the highest bit is still 1, and its exception code from 0123 up to 11, is actually related to the concept I just talked about. Take a look at this. For example, this part, it is divided like this. That is to say, this block here,

it is all software interrupts. This block here, is all timer interrupts. This block is all external interrupts. Now, these software interrupts, they are further divided according to the privilege level at which they occur, that is, when the interrupt happens,

the privilege mode the hart is in, so they are split into User, Supervisor, and Machine. And it is the same for the ones below. Each category is again divided into three. User, Supervisor, Machine. That is how it is divided. But regardless,

these three here are actually software interrupts. And this one here is the timer interrupt. This is the external interrupt which corresponds to these categories here Now, in terms of physical wiring that is, physically speaking

you can think of it this way this hart, as a CPU, actually has three pins, each hart has three pins, connected to these three types of interrupts respectively. For example, this figure here is one I took from this FU540, a diagram taken from that chip.

The emulator we use, that QEMU, this emulator was actually developed with reference to this real device, it was developed based on it. So we often also refer to the schematic of this device as a reference. Let me take some diagrams from this RISC-V reference material.

and use them to give you an introduction. Now, as you can also see in this diagram, when it was drawn, one hart has software interrupts connected to it, timer interrupts, and external interrupts — three categories. In fact, they come in through three pins,

three separate, independent interrupts. Now, this slide of ours, is actually focused on this external interrupt, that is, the peripheral interrupt. Let's take a look at this peripheral interrupt, and what the handling process looks like.

Now, whenever it comes to interrupt handling, we also run into a set of registers here. This diagram of ours, is really the earlier one. So up here in this one, we already have quite a few counters listed, and we've already covered all of them.

All of them have been covered Now there are two left These two right here These two counters we haven't talked about yet And we haven't covered them These two counters of ours are both related to interrupts So, that's why we want to briefly mention them here

Let's first take a look at this one give me just a second here Let's first take a look at one called MRE which stands for Machine Interrupt Enable Now this register here is actually used to control those three types of interrupts we mentioned

namely software interrupts, timer interrupts, and external peripheral interrupts Now, as we mentioned earlier, that mstatus register also has a global interrupt enable, so that is a global interrupt. In other words, for the interrupt in mstatus,

it's mstatus, doesn't it also have that MIE flag bit in there? So this is a this is something you can think of as a global interrupt. Once this thing is turned off, basically all interrupts, no matter whether you enable them or not,

are completely disabled. So, but then, this is divided into several levels. The first level, is this MIE, which is the global interrupt enable. So after this interrupt is enabled, we can also for this External, Timer, and Software

these three categories of interrupts, we can also set them individually. That's actually what it means. So MIE, think of it as a second-level interrupt control, control flags, control switches. Similarly, it also corresponds to, that is,

it is also divided into U, S, and M, each category is divided into these three types, U, S, and M. This is MIE, the role of this register. Then there is one called MIP. This MIP, actually this, you can think of it as being used for writing.

is that it is used to set things up. Now MIP, on the other hand, is actually used for reading. It actually by reading these corresponding flag bits in it, we can find out, in the current different modes, whether each of these interrupts has occurred or not.

If it is 1, it means this interrupt has currently occurred. If it is 0, it means this interrupt has not occurred. So that is essentially the role it serves. So these two registers, which are related to interrupts, are ones we need to understand.

Now, the interrupt handling process, in fact, is similar to what we covered earlier, so we can go through it a bit faster. That is to say, it is still divided into several steps. Among them, when an interrupt occurs, The HART will automatically do the following to execute

Now, over here, the only thing to pay attention to is which step is a bit different? That is, compared with exceptions, which step is different? It will actually take the value of the instruction after the current PC, and copy it into NPC.

The reason for this was actually explained earlier. The main effect is that, because when an interrupt occurs, what effect is it actually trying to achieve? The idea is that this normal control flow, it has been interrupted, and that's what we call an interrupt.

So after it's interrupted, once the interrupt handling and execution of the handler function is done, When it comes back, it can pick up right where it left off, rather than from this instruction. Actually, when executing this function,

this instruction has already finished executing. After this instruction has finished executing, it will then go and execute the next one below, this interrupt handler. Now, when it comes back, it expects to continue executing from the following instructions.

So it actually automatically takes this, the address of the next instruction, and copies it into here. That way, after it comes back, it can go on and execute the next instruction. So this whole normal execution flow, if you don't care about interrupts,

you can think of this process as actually not having, it's still actually executing sequentially, with no interference at all. but it actually gets interfered with it actually gets interrupted, I should say otherwise, there is really no difference from the exception handling we covered earlier

so I won't repeat it here and then, that is to say, the return is the same as well so this handling, in fact, there isn't much difference at all alright, this processing flow, we'll just cover it briefly up to here so, next, we're going to introduce

a component related to interrupts, called the PLIC such a component a device now, as we know, because what we're talking about now is external interrupts software interrupts and timer interrupts we'll cover those in later chapters

let's first look at external interrupts So, so-called external interrupts, external interrupts actually refer to interrupts generated by peripheral devices. That is to say, we've seen this diagram several times before. It means that on our bus,

this CPU and these external devices, they're all actually connected to the bus. So this external CPU, based on the instructions placed in memory, just executes them sequentially, executing and executing and executing. Now, during execution,

suppose this peripheral device, if it has something it wants to notify our CPU about, it can do so through an interrupt, to notify our CPU. Then the CPU's execution will be interrupted, and it will then go and execute, go and respond to this peripheral device.

In fact, it achieves this kind of effect. Now, there are many kinds of these peripheral devices. So these peripherals can all affect the execution flow of our CPU. But as we know, based on what we just introduced, for external interrupts,

our CPU actually only has a single input pin. That is to say, there are many peripherals, and we can't possibly take one interrupt line from each peripheral into the CPU, right? Besides, the set of peripherals changes often, and there may even be hot-plugging.

If we brought in one pin for every single peripheral, then the CPU design would become far too complex. So in practice, our CPU has only one input pin for external interrupts. So what do we do? This is where a conversion comes into play.

and aggregation is a problem So that's why we introduced a device called the PLIC. This device, you can think of this device as basically a Hub. Its full name is Platform Level Interruptor Controller, which is a platform-level interrupt controller.

It is an interrupt controller. Now, why is it called Platform Level? We talked about Platform earlier. You can understand it as basically an SoC, right? So it is a type of interrupt controller that is integrated into the SoC.

of interrupt controller. Of course, there are other types of interrupt controllers as well. What we mainly introduce here is the Platform-type interrupt controller. Now, its main function What this picture shows is that on the left side here,

there are lots and lots of peripherals. Right? There are lots and lots of peripherals. So coming in from that side, on the left, you have lots and lots of pins coming in, like a hub, all hooked up together. But over here, going out,

for a single HUT, there is only one pin. Of course, since there are quite a few HUTs on this side, it can, for each HUT, route a separate line out. But if we look at just one HUT, In fact, it's just like a multiplexer that is, there are lots and lots of lines here,

but over here, only one line comes out So this PLIC, it's actually just like a multiplexer. It, on its left side, faces a whole bunch of external devices, while over here, there is only one line. So what it does in this, its main function,

is really just, as everyone knows, the CPU, it can only, handle one kind of interrupt at a time So what it does here is It just acts as an agent. Based on the type of interrupt here, or some other things, which we will talk about,

like its priority and so on, it can filter them out and let only one current interrupt through. If there are multiple interrupts happening at the same time, it will only let one interrupt through. And then, for the ones on the left,

it coordinates all the interrupts, and on the right side, it serves one hart, essentially playing that kind of role. This is the interrupt controller. Why did we have to invent the interrupt controller? The main purpose of this kind of device

Now, on the left, we have lots and lots of peripherals. If every peripheral has its own interrupt line, then this, in terms of terminology, we call it an interrupt source. Because all interrupts originate and come from these peripherals,

so we call them interrupt sources. For every interrupt source, we give it a code number. We'll cover the related concepts later on. So these terms, I'll introduce them to you as well. So this is called an interrupt source. Now, in RISC-V,

in its specification, what it specifies is that, the current rule is that, these interrupt sources There are 53 of them defined in total, as standard So its value, it goes from 0 to 53 But number 0 among them, this interrupt source,

it is not actually assigned to any specific peripheral, but is reserved, reserved and unused. So the actual valid range of interrupt sources, is from 1 to 53. Now later on, aren't we going to show you an example, the one with the serial port device?

Well, the serial port device, it also has an interrupt line coming in, since it's a peripheral too. Now, this interrupt source of the serial port device, right here, We define it as a level Just so you're aware of it. This is what we call an interrupt source.

Now, this thing, the PLIC, is itself a peripheral, it is also a peripheral, right? So, if we want to access, or rather, to operate this kind of peripheral, of course we also do it through its registers to access it. And this, likewise,

is the same as the UART we talked about earlier, that is to say, all peripherals, they all use memory-mapped I/O, to let us access their registers. So, according to the definition in our emulator, the range of all of its PLIC registers,

is specified to be within this range, namely 0XC followed by 6 zeros so this starts at this address and then there's this range of addresses after it used to allocate registers for it Now the diagram on the right actually shows exactly what I mean

That is, this is basically a peripheral Last time we wrote UART here, right? Here it's the PRC The range of some of the PRC's registers is allocated within this range For details, you can refer to this code So its base address is this one

PLC This just defines a base address So which ones are there? Which ones does this PLC have? Which interface registers does it have for programming? So here, let me first introduce to you, the first thing, as we mentioned earlier,

this PLIC, one of its very important functions, is that based on the priority of the interrupt, it can select one to come through. So, we can actually handle multiple if there are multiple devices, multiple peripherals, then the priorities of the interrupt sources of these multiple peripherals,

we can actually configure them individually. We can set them, and how do we set them? Through a register called priority, for example. Now this register, its address starts from base, and then based on the interrupt number times 4

Because each register is 32 bits wide, which is 4 bytes. So you use that as the offset. Each PLIC interrupt source corresponds to one register. So if your board has, let's say it has fifty, fifty-two or fifty-three of them, then it's just one

one register per interrupt source, that's basically the idea. That's one concept. The second one is, the actual value you configure, it's a number, and that number ranges from 0 to 7. Among them, 0 means if you set this interrupt source,

if you configure it to 0, it effectively means this interrupt source cannot be used it cannot be used and the value goes from 1 to 7 1 means the smaller the value, the lower it is 1 is the lowest 7 means this interrupt source has a higher priority

so the PLIC, based on the priority of your interrupt source, it will make some decisions and of course, if the priority is higher, of course it will handle it first and submit it to the CPU for processing and you might have configured two interrupt sources

with priorities that are the same, right? so if they are the same, according to the specification, it will further distinguish them by the ID value of the interrupt source if the ID value is smaller, the priority is higher that is how it works

so it is essentially a two-level decision and handling process This is priority handling. Now, in our code, here we've also listed some of the related content in our code. It's actually this code. It's actually based on the concept we just covered.

First, we defined a macro, called PIC priority, and its parameter is ID. ID is the ID value of the interrupt source, the number of the interrupt source. Then, this logic below, actually, this piece of code, what does it actually mean?

It actually means that, URLQ, this thing is actually, the interrupt number of our UART device. This was earlier, it's right here, right? It's already here. I've listed them for you as well. So if we set it to 1, what does that tell us?

It means we take this UART0 device peripheral, and set its priority to 1. And 1 is actually a relatively low priority. That's what it means. This is the priority setting. Now the next register, it's called pending. The pending register,

it is actually used to... through this, when we read this pending register, we can find out whether a particular interrupt source has occurred. That's actually what it's used for. Now for this pending register, each PLIC actually has two of them,

because each one is 32 bits. But as we know, with 32 bits, it can represent at most 32 interrupt sources, right? But as the specification says, there are already more than 50 of these, right? 53 of them. So one register isn't enough.

That's why it defines two registers to represent them. Each bit represents one. So, for example, the first one, bit zero of the pending register, represents interrupt source number 0. Then bit one, represents interrupt source number 1.

So each one represents a single source. If this value, if each bit is 1, it means the interrupt has occurred, and is pending. If it is 0, it means that interrupt source has not occurred. That's actually all it means. So This pending register

can be both read and written. Through this claim mechanism, this claim method, I'll explain it to you later, you can clear its, this pending bit. The pending bit. Now it also says here, bit 0 corresponds to... Because interrupt source number 0 doesn't exist,

so this value, writing to it is actually meaningless. And the value read out is always 0. Okay. This is the pending register. Then, we can also control this interrupt source, whether to turn it on, enable it or disable it, ah. So this

as we said, this um, this register, it's one hart, one of these, ah, one hart, one hart, two of them, one hart, two of them, and the reason is the same, that is, because because a single Enable register can only handle 32 of them, so you need two to handle 53 of them.

But there are two of them for each hart, because it's configured per hart. Similarly, one bit controls one, this part is pretty simple. Just get a general idea of it. This is for controlling the interrupt sources. a register for enabling and disabling them

Now, our related code also the reference code, which you can take a look at also defines a macro So this parameter takes a current hart ID and then we set the UART interrupt source So these many bits are actually what we just talked about

each bit corresponds to one, based on its number, or rather its corresponding bit position This is the enable register And then there's another one called threshold that is, the priority threshold We can, for this hart, set the priority threshold for interrupt sources

So it's actually used to control Think of it like a threshold bar So if you set this threshold value If this of yours Because as we said, the interrupt source itself We can set its priority If this priority Is less than or equal to this threshold value

Then it's essentially like saying This interrupt happens It will be discarded Now for this threshold value If we set it to 0 Then it essentially means that As long as the original priority of your interrupt source Is a non-zero number

Then basically it can be accepted But if you set this threshold value to 7 Then it's essentially like saying All these interrupt sources will just be discarded That is to say, through this setting this threshold can further control

filtering out certain filtering out certain interrupt sources based on this priority Here is a relevant example of the code for handling this. Just take a look at this. So, earlier, these were just registers that control various switches

and thresholds. Now there are two more. One is called claim complete. Let's also take a look at these two registers. These two are, as we said, these two registers, first of all, what we need to understand is that these two registers are actually the same register

— their addresses are actually identical. However, the reason they are kept separate is mainly because the effects they produce are different. So we just gave it two names. Now, what about Claim? It's actually a read operation on this same register.

And the write operation on it, we call it Complete. That's what it means. Now, the read operation, we call it Claim. What Claim actually achieves, is that it can get the ID of the highest-priority interrupt source currently pending.

After a successful Claim, it clears the corresponding pending bit. And Complete, basically is like saying, we notify the PLIC through our program that we've finished handling an interrupt. Because as everyone knows, once this interrupt happens,

it still has to end up in our interrupt handling function. So how do we notify this PLIC? Is this interrupt handled? Yes, it's handled. We just use this Complete to write to this Complete register, and that's it. So the PLIC, actually, through this mechanism,

it can as you know, it's possible that multiple interrupts occur at the same time. Then it may hand the first interrupt to us, and after we've finished handling it, when does it hand us the second interrupt? It actually depends on us notifying it,

through this complete mechanism, notifying it when the second interrupt can be let in again, right? This is what claim and complete are about. the effect it is meant to achieve So basically, these are some related functions. Here I'll just

you can combine the concepts we just covered and let's take a look. We've wrapped all of this up, so basically regarding the programming interface this PLIC provides, how do we control it? How do we use this interface to interact with it?

That's where we'll leave it for now. So next, we'll mainly use this diagram to understand how exactly an interrupt occurs, and what the process looks like in between. We set these counters, and what role do they actually play? Now, this diagram

Actually, I copied this diagram from the PIC specification, a figure I took from the spec. We modified it slightly, to illustrate the operating flow of the PIC. Now, on this diagram, first, let's get a sense of what it shows.

What's being conveyed here is that this is a PIC, this whole thing is a PIC. Now, up here, for the sake of example, let's say it has two external devices connected in, two lines — one is called 1, the other is called 2. You can think of them as two peripherals.

Then over here, it is actually connected to two CPUs, right? Two CPUs coming out. This one is actually considered to be a an interrupt line for external interrupts So for a CPU, there are actually two interrupt sources. But there's only one line here.

And then, this actually reflects the concept of those registers we just talked about. This is the priority setting. That is, when we set the priority, as for that priority, don't we set it for individual interrupt sources? Well, this thing here actually corresponds to that.

It's the setting, the setting. This is used to control the priority of interrupt source 2. And this is used to control the priority of interrupt source 1. Then as for these things, RE actually stands for enable, corresponding to the enable registers.

This register, the register where we actually do the enable, depending on the different HART, we can individually configure these different interrupt sources separately. As you can see, right? For example, this these two are for the two on this HART,

and these two, again, are configured for this HART. This is the Enable. And then this one is for the Stretch Hold, that is, the threshold, setting the threshold for each interrupt source, setting the threshold for each HART. It should be the threshold for each HART,

right? Let me take a look. The threshold is set for the HOT level. The threshold is for the HOT level. So this is a threshold for HOT. This is the threshold. And then, this part here, is actually the Pending bit. That's what I use

by reading it out, we know whether this interrupt has occurred or not. And this one here is Claim and Complete. Claim and Complete. We go and read this claim and complete, and go write to complete, which is this register. So in this figure,

all the registers we just introduced, they're all covered here. Let's take a look. Let me give you an example. Suppose we have two interrupt sources. After these two interrupt sources occur, what happens to us? How we set these registers,

what effect will that have? Now suppose we set the priority of interrupt source 1 to 1, and the priority of interrupt source 2 to 2. What that means is, as we know, the larger this value is, the higher its priority. So basically, interrupt source 2

has the higher priority. And then, for example, if we also set For Hart let's say for Hart 0, we enable both of its interrupts. And on this side, we disable them. Now once this is disabled, as you all know, nothing can actually get through,

Hart 0 can't get through here. And then, we also go and set this, its threshold. The threshold, for example, let's set it to 0. Setting it to 0, as we covered earlier, means it effectively lets all interrupts through, lets all interrupts through.

Okay. So suppose this peripheral 1 and interrupt 2 occur. Now, of course, the example we're using here is one where they happen at the same time. But happening at the same time, doesn't mean they actually come in at the same time.

That's something you all need to keep in mind. So, so first, we judge them based on priority. We know that priority 1 is lower than priority 2, so which interrupt gets in first? Well, it's definitely interrupt 2 that comes in first,

right? Besides, this path is open, so it can come in. The path below can't come in. In other words, this second one, the Hot —it can't get in here, because this path This path is currently closed off. It's closed off. It's been disabled.

Meaning this can't get through. Now, once it does get through, this point, because the Threshold is also 0, so 0 is definitely less than 2, so it can definitely get in as well. So once it gets in, then, after it gets in, we say that this source 2,

it's basically as if this interrupt lights up. Once this light comes on, the light coming on is basically like saying, the interrupt on our CPU side has occurred. It will then, the execution process on our CPU here will get interrupted.

So the interruption takes us into our interrupt handler, and we go and handle this interrupt. Now, as for the handling process, Actually, what we need to do is figure out which source came in. We do this by calling claim, and by calling claim,

we can find out, because we know that after the claim call, it returns the ID of that interrupt source. So then we know, for example, say this is 1 2, so it's number 2. Once we know from claim that it's number 2, we go into the switch-case logic,

right? We handle the logic for this interrupt source number 2, and once that's done, what else do we need to do? We need to One thing to mention here is that if we do a claim then this this pending bit will actually be cleared this pending bit will actually be cleared

And then, after we're done handling it we call this complete operation Now, complete actually means going to going to write to this complete register So once we write to it, it's essentially like saying we've notified the PLIC

that this current interrupt has been handled After it's handled, then as you can imagine, interrupt 2 is done Once it's done, then of course, as you know, interrupt 1 actually this part is blocked right here and never gets in right?

Then this interrupt number 1 will come in. So over here for number 1, the Pending bit lights up again. And once it lights up, then our in effect, our CPU can handle interrupt number 1. So this, in a nutshell, is how PLIC does its arbitration.

That's how it works for us. So just get a general idea of it. Now, the last part of this chapter is to give you an example. That is, after we've learned how interrupt handling works, Let's take a look at how we can use some of the related knowledge about interrupts

to implement that is, still using the UART device because as we discussed earlier, we used the UART to implement an output. Now, through interrupts, we're going to implement an input. What does that mean? It means that when we're on the console,

say we type a character, it actually goes through the keyboard, and then through this console, it sends it to that serial port. In other words, the data gets sent to this serial port device. Then this serial port device, it will essentially,

it will get a it will generate an interrupt to notify this CPU, right? Let me take a look at this. Let's go over the principle of this briefly first. That is to say, we know that let's say this is our CPU, and then on the outside,

it has a PRC connected to it. As for the PRC, there's a UART device connected on the outside. This UART device, and this one here is the P-L-I-C, this thing right here. And this one here, maybe this is our HOT, the CPU. Now, this UART,

this thing here, is actually all essentially inside our SoC, this platform, right? Now this one, if we're talking about a real physical device, it connects through this serial port device, and hooks up to a host machine of ours.

I've drawn it kind of small, but in reality this thing is a bit bigger. Now this host machine, through this side here, through the keyboard, this side is the console. So we type a character C here, let's say we type a character C,

then this character C, goes through this serial cable, it gets transmitted right up to this point Now, how does this UART take this it receives this character C, how does it notify our CPU? It actually does it through this, it sends out an interrupt

and then our handling flow goes like this: once the interrupt comes in, it knows a character has arrived, and then we can take this character and read it out from this, and after reading it out, we use the output we implemented before,

take this content, write it through this, write it back, and then through this serial line, write it out again, and on the screen we will see a C appear. That's how the whole flow works. What we actually want is to use this kind of

flow like this So the one just now, this line here, is actually an interrupt, it notifies us through an interrupt. Now, for writing it back, actually, writing it back, it reuses our original logic from the very beginning, which is the polling approach,

and then writes it out. This way, we see a C on the screen. Let's first take a look at the effect, and demonstrate this effect, so everyone can better understand what I mean. Here, we enter the sixth interrupt example, which we have already compiled.

So let's run it. Now, everyone can see that this task is actually still running. But on the keyboard, for example, I type a C. When you see this C, it triggers an external... it triggers an external interrupt, right? Let me stop this.

Now I press a C again. If I keep pressing it, it will keep triggering interrupts. So this... this line being printed out means that if... if this line gets printed out, that is to say, this line, if it gets printed out, then it shows that this...

interrupt This UART, after it receives this data, the interrupt is generated. This generation, and then it takes this C, reads it out from the interrupt, and then echoes it back. This is actually done by writing it out. We achieve this effect.

Let's take a look at how our code implements this. So first of all, for this PRC, we need to do some corresponding initialization for this peripheral. Actually, this is all implemented in the PRC, in this init function. Now, these things,

based on what we discussed earlier, this is setting its priority. Setting it, whether it's enabled. It's its switch, the hold. Right? So what are these two steps doing? These two steps are exactly what we talked about earlier.

That is, this MRE register. And the MSTATE register. As for this thing, actually our MRE, as we mentioned before, it can control As we said, interrupts come in three kinds. External interrupts, software interrupts, and timer interrupts.

We can enable them individually. So what we're actually doing here is taking this E, which stands for external, meaning external External interrupts. We did an enable. At that time, everyone should pay attention to how it is written.

It first reads out the value in this register, then sets the corresponding bit, and then we write it back. That is actually the process. This ensures that the values in the other registers will not be modified by us. Now this is actually a global interrupt enable.

This is actually the global interrupt. So for this interrupt, as you can see, it goes level by level, with many levels. This one is global. And this one is the three main categories. Software interrupts, timer interrupts, and external interrupts.

You can also turn each of them on and off separately. And then here you can also use this And here there's also, for the interrupt source, setting this HART up with a PRC-level enable Right? And then here you can also use the threshold register,

to filter the interrupt sources. So as we can see, this whole interrupt handling process is actually divided into many levels. At each level, we have quite a few stages, and at every level we can control whether this interrupt gets handled or not.

This process is still a bit complicated. Now these two lines refer to claim and complete, Let's skip that one. This is inside the prc. We'll set up the code here. This is our one... In this step, we'll add this new file. Now, there's some processing done in here.

Now, as far as day-to-day work goes, as far as this trap goes, inside the trap handler, what we actually do is this switch. First, we distinguish whether it's an interrupt. If it is an interrupt, since the highest bit is 1, that means it's an interrupt.

For interrupts, we go by this cos, that is, the mcos that was passed in, this value, this value here. For 1234, when it's 11, This value should actually be I think we covered this earlier, so let's take a look. Right here, it corresponds to this entry in this table:

1234. So now we should have an external interrupt, so it should be this block. So we should have a machine-mode external interrupt, so this value is 11. This one is 11. That's too much, so over here it's 11. Now, if it's 11, as we know,

then it's an external interrupt in machine mode. So at this point we just enter this interrupt handler Now, in this interrupt handler, we can see the processing flow. The first step is to claim. As for the claim process, as we talked about earlier,

claim is done through its function, which returns the number of an interrupt source. Now, this interrupt source number, based on what we know now, since we are handling the UART, we check that if this number is the UART, then we handle one

and enter the UART's handler function. We enter the UART, and then after the handling is done, please note, don't forget, you have to call a Complete. This way, we can essentially notify the PLIC, that our handling is done, so it can let the next interrupt come in.

Pay attention to this, don't forget it. Now, in the UART logic, what we need to do is initialize it. In this initialization part, we need to add one thing, which is to take this interrupt controller inside our UART, and turn it on.

Please note this. And also, its interrupts are split, since you know the UART is split into two lines for receiving and transmitting, so what we control here is the interrupt in the receive direction, because right now we are

for this example, what we actually want to receive is a piece of data sent from the console. As we have already explained to you earlier So this function Is exactly what we just saw It just got dropped in all the way down Right

That's exactly how it gets dropped in So once it drops in The logic inside here is very simple Once we can make it here That means the character has already come in The character has already come in Then we just need to use a getC function

To read it out Now this getC function Is implemented right here The related logic here Why is it done this way I won't explain that Go back and take a look at it yourselves So after we read out a character Based on what we just explained,

we'll write it out again, write it out again with putc. So once it's written out, our controller will write it to our console. So on the console we see the character c appear. That's how it comes about. Right here we've written the character out,

and after writing it, we output a carriage return, and continue on. This is the example I just demonstrated to you. That's how it works. I'll stop here for now. As for the exercises on this interrupt, go back and work through them yourselves.

All right, now regarding this external interrupt, That's all we'll cover for you here.
