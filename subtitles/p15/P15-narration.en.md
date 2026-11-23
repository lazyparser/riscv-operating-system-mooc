# English Narration Script

Chapter 6. From Chapter 1 to Chapter 5, we've basically covered all the prerequisite material for you. That is to say, from here on, we're going to start to actually write a try our hand at writing an operating system from scratch.

Now, before writing an operating system, there's a lot of knowledge we need to lay down first. That's what we covered in the earlier parts. So you can see there really is quite a lot of content involved. Why do we say writing an operating system is rather hard?

Well, it's because of all the basic knowledge you have to master beforehand, and there's quite a bit of it. So getting started is not exactly easy. So from Chapter 6 all the way to the end, what we're going to do is write a small, simple operating system for you.

And through this study, you'll get to know a bit more, and go deeper into some of the RISC-V details. especially privileged instructions and some things in the privileged architecture Let's first take a look at this Chapter 6

Chapter 6 is actually still a kind of introduction to our second part Let me quickly walk you through it roughly four parts namely the definition of the operating system operating system classification typical RTOSes and a brief introduction to the course project

So this covers just this little bit I won't go into that the reference manual As for the operating system, we mentioned it earlier, and we'll review it here That is, the operating system, which in English is called Operating System

which is short for OS and so it, is actually also a collection of software, right? We say that the operating system, actually also branched out from software. The part that floats upward, became application software, and the part that sinks down became the operating system.

Now this diagram, is also a figure we found on Wikipedia, and you can take a look. It shows where the operating system sits within the overall software system. That is to say, our operating system, including this, especially the operating system in the narrow sense,

namely the kernel of the operating system, is actually sandwiched between it actually includes this the driver is also part of the kernel so this, between the hardware and the application program the application program is on top

and in the middle through a system call right this diagram should be a classic one to all of you so as for the operating system we say there is a distinction between the broad sense and the narrow sense so in the narrow sense it is really just the kernel

that is what we usually talk about things like Linux that Linux thing is really just a kernel right There are many kinds of operating systems in the broad sense. For example, what we call Ubuntu, and things like Debian, right? Now, these things,

we also call them operating systems. But actually, that is a broad concept. In other words, it is really a distribution package. You all should know about this, right? That is to say, take Ubuntu for example, it is nothing more than a Linux kernel,

plus some upper-layer software, installation packages and so on, put together. It is actually a distribution package. But in the broad sense, we also call it an operating system. Our Windows is also a broad-sense operating system.

Because Windows also has a kernel, it is just that we probably don't really, it is not open source, so we don't really look into it much. But it also has this concept inside it. So that's the basic concept of an operating system.

Now, when we talk about operating systems, based on certain application scenarios, we can divide them into a few major categories. One classification puts them into three major categories. The first type we call a bare-metal system.

This kind of system, it's very small, and it doesn't really have any layering. It mainly runs in some what we'd call small embedded small-scale controllers, like microcontrollers, for example, where it can only control some simple peripherals.

So when we talk about these three categories, we can actually also take the opportunity to review what we discussed earlier about RISC-V, namely the concept of so-called privileged modes, the concept of privilege levels. This diagram,

that is, the diagram below, I think you still remember it. We talked about it earlier. That is to say, in RISC-V, it takes these privilege levels and divides them into, currently, three major categories: Machine, and Supervisor, and User Mode,

right? Still remember? Now, at the time, in this diagram, he also mentioned this concept. In fact, it corresponds to ours. For example, this logical system, let's just use this diagram and go through it together. Now, for the logical system,

as we just said, it is a kind of what actually corresponds to our, that is, the kind of thing described here, a simple embedded system, is what we call a logical system. Now, for this kind of system, it actually is just, we actually just directly

only need to implement our RISC-V's, machine mode, and that's actually enough. Now, the second category, it is also mentioned here, that is, it includes M and, machine mode and this user mode. Now, it, the main difference from the one above,

is that What it mentions is that it is a kind of embedded system with a certain level of security built into it. So what does that mean? This thing is actually somewhat similar to what we commonly call an RTOS, that is, what we refer to as a

real-time operating system. Now, for this kind of system, what it has so-called a certain level of security is reflected in the fact that it can distinguish at least two modes: one is machine mode, and one is user mode. In other words,

Ordinary applications, they run in User mode. The kernel, including some low-level drivers, may run in Machine mode. Now, in User mode, our access to the underlying layer, it is protected. We cannot directly access certain special physical devices,

including memory. Under these circumstances, in general, we would need the processor to support what we mentioned for RISC-V, a feature called physical memory protection. Of course, it doesn't have to be supported. But having it is of course better.

Now, the real-time operating systems we often talk about, most of them fall into this category. In that case, And the third case below, the main difference is that let's look at the third one. The third one corresponds to what RISC-V calls

an operating system that combines all three modes. Now, for this kind of system, he believes the main point is this: the typical application, that is, so-called Unix-like operating systems, systems like Linux, FreeBSD, these kinds of systems.

For this type of system, the main difference from the ones above is that it adds one more level in between, called Supervisor Type. This Supervisor Type, in other words, when it enters Supervisor Type, the most crucial thing is

The most important difference is that it introduces the virtual memory mechanism, that is, virtual addresses so it can support the concept of processes It can separate this virtual address from the physical address, and above that

this mode and the mode above it In Machine mode, it actually accesses memory directly using physical addresses including some modes here they all use physical addresses for access Now, this is what we call the classification of hypervisors

roughly like this In this course we actually focus more on the two levels above In this course since our time is limited we won't get into concepts related to very complex advanced hypervisors In this course of ours, our time is actually limited,

so we won't get into the more complex advanced hypervisor concepts. We actually still build on top of those two kinds. So this one. So let's take a look at what some typical RTOSs are. This is just something for you to be aware of.

There are actually quite a lot of RTOSs out there. If you search for them online, you'll actually find quite a lot. So I'll just briefly introduce the common ones to you. For example, one of the more famous ones is called FreeRTOS.

I don't know if you've heard of it. FreeRTOS is very popular. It seems that nowadays Amazon or whoever it is uses it quite a lot too. When they're developing some small devices, they tend to prefer this kind of thing. It was created back in 2003,

and it uses this kind of MIT license. This license is also quite flexible, and fairly open as well. The design is also quite compact. Its entire core code is only about 3 to 4 C files, so it's very easy to read and understand

the general functionality it implements. Maintainability is very strong, since most of it is written in C, so the readability is quite good. But for an operating system, there are still some parts that cannot do without assembly.

So assembly is also very important for us. Now, it implements some basic features of our operating system, including, for example, multi-threading, mutual exclusion, semaphores, and these concepts we will also mention later on.

Its support for architectures is also quite good, for some of the popular CPUs we use. They all support it. For example, ARM, x86 and such, RISC-V is also supported. And it supports quite a number of microprocessor processors.

Now, this RTOS is from abroad, a fairly typical one. Then, for the corresponding one from our domestic side, let me introduce it to you as well. Have you heard of the operating system called RT-Thread? It is also a real-time operating system,

basically corresponding quite well to the FreeRTOS we just mentioned. Its application scenarios are also similar. As for when it came into being, it was slightly later than the previous one, around 2006. The license is Apache.

As for the specific features, I have also listed them here. This is actually copied from its official website. I won't read it out here. As you can see, for this kind of typical RTOS, they're basically all like this. And as for RISC-V,

it also has a certain level of support. So those are the two we covered earlier, which are fairly typical. Now this RTOS, is actually the system we're going to develop ourselves today. I just gave it a name, so it's easier to refer to.

It's not some real external, actual project out there. Actually, it's just our— did I write this link wrong? I messed it up. I'll go back and fix this link later. Sorry about that. This link seems to use rCore-Tutorial That's not right.

Skip this one. Actually, this thing... It's actually our... the little system we wrote ourselves. Well, it... is really entirely for... our current teaching and course development. It was born in 2021, which is this year. In fact, I only wrote it today.

It took me some time. To teach you this course, I just wrote it myself, simply. As for the license, we use BSD. This license is pretty good too. Basically, it means everyone can use it freely, though of course it might not be of much use.

And so our code is quite small. Actually, our lab has limited functionality, because we are not trying to build actually trying to build a truly practical system. It is entirely for the purpose of learning some RISC-V stuff, and we also

learn how to develop a small system of our own from scratch. So as for the code part, we use C and assembly, and then, below, what follows mainly involves content that we will cover in the later parts of this course. The first part,

we simply demonstrated some basic memory allocation, how to implement some memory allocation. Now here, since we are not dealing with virtual memory management, so our approach, compared to, say, Linux, is quite a bit simpler. This is also to

reduce the difficulty of learning for everyone. Time is still limited here, and we can't possibly spend too much effort discussing because memory — the whole memory allocation area it's a very complicated piece of things. Once virtual memory comes into play,

a lot of things become very complicated. So for now, we've cut this part out. Then if there's a chance later on, we can come back and introduce it to you. Now, the second thing is, as far as a so-called hyper system goes, a very core function

is to implement the concept of multitasking, that is, multithreading. And we do support this as well. Moreover, we've also implemented the concept of preemptibility. You can all go check it out; we'll introduce it to you later on.

Now, once we have multithreading, that brings in the concept of task mutual exclusion. So on that note, we'll also cover that for you. And then, another thing is that in our operating system, an essential component is the timer.

Now, we have hardware timers, and we also have software timers. We use both of these here. So here, we also gave you all a simple demonstration. As we just said, this M plus U is the kind of mode with a certain degree of security,

that is, the M plus U mode offers some protection. In other words, even though we haven't implemented virtual memory, we can still achieve some basic protection. That is, by splitting things into layers internally and using system calls to achieve it.

And then, one more thing to mention is that since we're using RISC-V, what we've implemented so far is RV32. Everyone understands this, right? That is to say, we're using 32-bit instructions as the basis for our explanation.

As for 32-bit, in actual practice, most things have already moved on to 64-bit. So, of course, some small CPUs still use 32-bit. Now, for teaching purposes, we chose 32-bit mainly to make it easier for you to read, because, as you know, when we do internal debugging,

we look at the memory inside, the instructions and so on. 64-bit is always a bit harder to read than 32-bit, because it's so large, right? 32-bit is more compact. So that was one of our goals in choosing it. And for our development,

since it's also based on a It would have to be based on real hardware anyway But to make debugging easier for everyone, we're actually using an emulator to do this. So the way we do it, the hardware platform we're targeting, is actually an emulator.

But you can imagine that this code, with just a slight modification, should have no problem running on real hardware either. If you have some money, or if time permits, you can also go and now that the RISC-V chips are out, and there are some small boards available,

you can go buy one. If, that is to say, you have the means, you can also give it a try. Alright, so about this So for this project, that's actually all we'll cover in our course introduction. I'll stop here for today. Let's take a break.

Then next, in the final lecture, we'll move on to our combined part of the course, alright.
