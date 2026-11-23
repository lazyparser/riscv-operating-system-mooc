# English Narration Script

Good afternoon, everyone. Let me first introduce myself. My name is Zhao Wangcheng, and I am an engineer from the Nanjing Institute of Software Technology at the Institute of Software, Chinese Academy of Sciences.

The course we are bringing to you today is called Learning to Develop an Operating System on RISC-V, Step by Step. That is our course. So first, let me give you a brief introduction to this course. It mainly covers a few parts.

First, I will talk about the purpose of taking this course. Then, second, the overall syllabus of the course. Then some prerequisites for this course. As well as an introduction to the online learning materials. And some reference materials.

So that you can get a general idea. Before talking about the purpose of taking this course, let me first quote our Premier Li Keqiang, who, just recently, at the Fourth Session of the 13th National People's Congress, just said this.

Right? You can take a quick look at it. This quote is very short, and it happens to be addressed to us, young students. I think it's quite interesting, and also very meaningful. So, borrowing this quote, I'd like to pass it on to all of you.

Among other things, the main point here is that, we need to strengthen what? The learning of foundational knowledge. Right? Building a solid foundation and fostering innovative ability go hand in hand. So, that is to say, everyone,

even though we're talking a big game about innovation these days, very loud and strong, if you don't build a solid foundation, then innovation is like a tree without roots. Right? Now, for this course of ours, the overall content

is actually more oriented towards, that is to say, to strengthen everyone's grasp of the fundamentals especially for those of us in computer science, these fundamentals mainly relate to programming, this kind of foundational knowledge.

So, that brings us to what comes next, which is the specific goal of this course. For this specific course, we have a few areas we'd like to cover, and we hope that through this course you'll be able to master them. The first part is

getting to know RISC-V. As for RISC-V, we'll go into it in detail later on. Basically, it's you can think of it as a kind of CPU, a processor instruction set architecture, just like ARM and x86. is a new instruction set architecture

Now, this instruction set architecture is currently developing very well and in China we are also promoting it vigorously So we will use this instruction set architecture as the core to introduce to you and together develop such a small operating system

First, to understand its related knowledge Second, in the learning process I will take you through some related technical manuals Because RISC-V is actually a standard it has related technical manuals And all of our learning content is developed based on this technical manual

And the third thing is because we want to develop an operating system besides basic C we also need to master some related things about the RISC-V instruction set mainly some assembly and I hope everyone can get to know it So the fourth part

is because OS development is actually closely tied to embedded development. So some embedded development skills through this course and the hands-on training we'll also be able to strengthen. And then finally, on top of all that,

once we've got it all down, we'll take you through step by step, from scratch, and we'll write a simple operating system. That's pretty much it. This is the learning goal of this course, what I hope everyone will be able to take away

after finishing this course. So next, Let's take a look at the content of our whole course. In terms of the total time, right now we've scheduled about seven sessions. As for the overall content, we currently have it written up as 16 chapters.

Give me a moment to bring up the one on the screen, so there are 16 chapters in total. As you can see here, 16 chapters in all. Now the overall content, we've divided into two major parts. One is the prerequisite knowledge, and the second part

is our main topic of study, which is operating system implementation. As for the prerequisite knowledge, actually, from the content above, the names should give you a good idea. That is, before we write an operating system, there's still some material

these are things you need to master first Now, this content for example, Chapter 1, A Tour of Computer Systems, is actually something that basically corresponds to the computer organization part of our computer science curriculum

I'm not sure if you've taken related courses before Let me get a rough idea. So, for everyone here, you have covered related content before, right? What did the "group-level organization members" study? Let me find out. Let me find out.

The students sitting in the front, you're all in the same class, right? You, the earlier group members, what did you study? You probably still remember, right? What architecture did you cover? Which instruction set architecture was it?

Do you still remember? x86, ARM, or something else? x86, right? Now on our side, from here on, we'll mainly focus on RISC-V. This is also a new architecture. So we'll go over its ISA — that is, the instruction set architecture — and give you an introduction to it.

Chapter 3, actually, is to introduce to you the content on compiling and linking. Chapter 4, mainly, is because as we just said, when you develop an operating system, It involves quite a lot of knowledge about embedded development.

So when it comes to embedded development, I'll cover some of that for you too. Now, Chapter 5 is also part of the first stage, the foundational knowledge, and it's one of the more important parts, namely RISC-V assembly language.

Since you've studied x86 before, you should have grasped x86 assembly, right? It's actually pretty similar. I mean, it's really just a different processor, and so its assembly instructions, we need to master them. Because when you write an operating system,

it involves some low-level handling, and that has to be written in assembly. So it's the fastest, and I hope everyone gets familiar with it too. So, chapter 6 starting from chapter 6, we'll begin writing our operating system. Before that,

you've probably used some operating systems, right? But actually writing an operating system from scratch, you probably haven't really done that yet. So what we'll do is take you step by step, writing one from scratch, a relatively simple operating system.

Not very complex. It definitely won't come close to a system like Linux. Now that is still very far away But then, by going through this course, we can also see that writing an operating system is not some unreachable thing, remember back in the day, Linux

wasn't it written while he was in college, right? He just used his vacation time at home and put together the very first version of Linux just like that. Right? Now, for this part, the content we've designed, mostly at the beginning,

is just a brief introduction, about RBOS and some other things. This RBOS is what I named the operating system that we built for ourselves I gave it a simple name. And the RB? That has something to do with RISC-V well, a little bit.

OS, as everyone knows, is just an operating system, right? I picked a code name, so it might feel a bit more familiar and friendly when you say it. Now, the first part, Hello RBOS, basically means we're starting from scratch.

So at the very least, we need to get a Hello printed on the screen, right? And then, gradually, in Chapter 8, we'll start talking about some memory management, because, as we know, when a program runs, you absolutely cannot do without memory.

So memory management, is a very important part. So, Chapter 9, actually, starting from Chapter 9 all the way to Chapter 13, it's really all revolving around one thing, one of the most core components of writing an operating system, which is task scheduling,

right? As you all know, this process thing, even though we... what we cover later on, actually we don't get into such complex process concepts, but the concept of task scheduling still has to be implemented. Now, task scheduling,

there are many ways to implement it, including cooperative scheduling, preemptive scheduling, We'll give you a detailed introduction to this later on. Now, in the process of implementing this task scheduling, we'll actually run into some

things that are very important to us, such as the logic of interrupts and exceptions. If you've studied some of the earlier material, or if you've had some exposure to Linux, you should have heard about interrupts and exceptions,

right? As long as you're working on kernel stuff, these things are all very important. And then going further, we'll also bring you that is, after we finish talking about task scheduling, if we have time, we'll also cover some synchronization between tasks,

and some software conventions, and some concepts related to system calls. Well, actually, for a course like Operating Systems, its content goes far beyond all this. But our time is limited, because we have a total of 26 class hours,

right? If we were to take you from absolute zero, and build all of this from scratch, including some of the prerequisite knowledge beforehand, then 26 class hours is definitely nowhere near enough. So we did trim down the content.

So, a lot of things were really still left uncovered. You can probably imagine — that is, if you've been exposed to operating systems What other things do you think we haven't covered yet? Feel free to just chat about it. If you find it interesting,

we might add some more for you later on. What do you think about the super system? Beyond these super system topics I've listed here, beyond what's related to the super system, don't you think there are still some super system things missing?

There definitely are. Right? There are a lot of things in this super system. Can you think of any? For example, the simplest one, say, the file system. Right? You've dealt with file systems, haven't you? All these things, they're actually already quite complex.

And since our time is limited, we'll probably wrap up here. But I think if you follow along with me, that is, if you work through this step by step and finish it, then later on, you can actually try a lot of things on your own.

Because as we say, the master leads you to the door, but cultivation is up to you, right? So this thing, once you've finished all these things, the operating system will no longer be some mysterious, unfathomable thing to you.

And then slowly building things on your own on the internet, I think that will actually be a better start. So that's the end of my introduction to the course outline. Today's time will mainly cover Chapter 1 and Chapter 2. Now some students say, isn't an operating system too hard to write?

Do we need to master a huge amount of stuff before we can write one? Actually, looking at what I've listed here, you'll see that to master an operating system, you don't really need an especially large amount of knowledge. Look, there are just a few items listed above.

The first one is, a reasonably solid command of the C language. Seems like a small case, right? I think this... And you're all computer science majors anyway. Even if you're not a CS major, many students learn C on their own. Right?

So if you can really master C, I'd say that gets you more than halfway to success. Then what else is there? Well, some data structures. And in our development process, for this kind of bare-metal code, we try to avoid using overly complex data structures.

So, for us, linked lists, if you can master them well, I think that's really all you need. And one more thing: our development environment. It's still done on a Linux development environment. So, I hope everyone can have some understanding of Linux's

commonly used commands and such. Now, beyond that, if you haven't learned some of these things before, we'll guide you through them. You all understand what I mean, right? As for other things, there aren't too many requirements.

That's about it. So it's still not particularly complicated. Anyway, through this course, what I hope is to let everyone reduce some of that... like the kernel, you all feel it's really complicated, right? to reduce that sense of

you could say it reduces that kind of fear a bit because when people first learn the kernel, you might think it's just too complicated but it's really not that complicated. So this... we... our... Besides covering this in class for everyone,

we have a... a lot of online materials that have already been put online. As for these materials, after class, you can go and download them. And our main mirror, is right here, on GitHub. PLCT Lab— that's the name of our lab, This is the name of our lab,

PLCT. So our course... This is the GitHub site. So this... If GitHub is a bit slow for you, then within China, this Git, it's Gitee, I guess, right? This Gitee page also has a mirror copy. So you can just download from there. The Git operations

are not covered in this course. So it's assumed that everyone still needs to know how to use it. That shouldn't be a problem, right? You should have Git operations all covered, right? Haven't you? If not, if you're still not familiar with it,

this command is very simple. Just search online yourself, and that should be fine. So once the code is downloaded, let me briefly walk you through roughly what it looks like. In here, is it a bit small? Once it's downloaded, once this code is downloaded,

you'll see a few directories here. That code directory is where some of the demo code we'll cover in the course lives all under this. Inside, there are two directories one is the ASM directory and one is the OS directory. So the ASM directory

is really just some since we have a part on assembly language to learn earlier on, right? So ASM is short for Assembly, and the reference examples for assembly are all in this directory. Now the OS directory is where we build an operating system from scratch, step by step.

When you click into it, you see 0123456, roughly from step one up to about 11 steps so far. We develop it step by step following these. As for the specific content, When we get to a specific chapter, we'll go in and take a closer look then.

This is the code. Then there's the refs directory, this directory, which contains reference documents. Reference documents. These are some important references, including some specifications, and other materials related to those specifications and so on.

They're all in this directory. And then there's also slides. The slides are our course slides. The course slides over here, have been converted to PDF, to make it convenient for everyone to view. And there's nothing much else.

The rest is just some readme files and things like that. This, this, this, this file is not there. So, uh... This is the course material right here. And then there's also the development environment we'll be dealing with, the development environment.

Right? The development environment, here's the deal. The development environment works like this. You open this, that is, after you clone this git repository, you can open the readme at the bottom to take a look. This readme, there's an English version,

and a Chinese version. Let's just use the Chinese version to show you. Um. I'll quickly walk you through it, that is, how to How to, how to install some things. For this, I'll just use... Can I get online here? Let me get on the, get on the internet.

And then just take a look. It might be easier to read the readme online. Just a moment. The network here seems a bit slow. Let's use the directory on Ma Yun. If your computers can get online right now, you can open it up and take a look too.

On this, look at this Ma Yun page. Down below here is basically a short introduction for us. He's already put it up so you can read it on the web page. So this mainly walks you through these parts. One is the introduction. Now for all the examples here,

as for my own verification, I verified them on Ubuntu. If you want to make things easier, Actually, ideally you should keep the same environment as ours I'm using Ubuntu 20.04 20.04 that version As for the kernel, 5.8 that's what we use

So if you want to install it manually, once you have this and want to install it, you may need to install some dependency packages first I won't go into that, you can look it up yourselves So the main thing is to download this thing

download an archive This archive contains the toolchain we need to compile things in our environment You've used things like compilers, those kinds of tools, right? You've used GCC, right? Yeah. So we've set all of this up for you here.

Inside this, there's a compiled GCC, plus the simulator environments we need when we run things, all under here. Now, this archive, it's a few hundred megabytes. I've temporarily put it on Weiyun, which is Tencent's cloud. Go ahead and download it,

and see if you can get it. Can you access it? If you're online, give it a click, and see if it's still accessible. I clicked it myself, and it should definitely be accessible. You can still access it. It can still get online here.

You can still go and access it. and take a look. After it comes down, it's very simple. You just need to extract it. Everyone can just follow these steps. So this thing, you really have to download it. Because all of our development tools

and runtime tools are inside it. So once that's done, if you want to verify whether everything is installed correctly, just go to our repository directory and run make. If you see that it compiles successfully, then basically there shouldn't be any major problems.

Just wanted to mention that to everyone. I'm worried some of you might run into problems setting up the environment Now, if you set it up yourself and it still doesn't work, we've also put together a foolproof foolproof runtime environment for you.

Of course, if you need it, you can come ask me for it. Basically I packed it into a virtual machine, and you can just use what... which one are you using? VMware or... VirtualBox and things like that? VirtualBox, right? What I made is also a

VirtualBox one, fully set up and ready to go. So this environment is available, but it's pretty large. It's about once extracted, it's about several gigabytes so it's a bit big so I just I have it with me, if any of you need it later,

I can bring it over and set it up for you. If you install it yourself, that works too, following my setup here. This one is pretty small, just a few hundred megabytes, and you just extract it and you're good to go. Alright, so that's basically it for this environment and the code,

so I'll wrap up the introduction here. Later, if there's time during the break, you can go ahead and try it, or after you get back home, you can also set it up yourself. Alright. That's basically all the online resources there are.

So, uh, finally, well, during the design of this course, we also referenced some pretty good resources online, and I've listed them here for you. Um, actually, that is to say, after you finish this course, if you still have energy left,

you can also go further and keep learning. So the first one is 'Adventure of OS'. This is from a foreign, a... I forgot which university it was. Some university in Europe or Australia, I think. Written by a professor there, also.

But it's not written in C. It's written in Rust. Have you all heard of the Rust language? It's getting a bit of attention now. But overseas, it's actually already developed quite well. They wrote it in Rust. But on our side, considering that

you probably don't need to learn yet another new language for this. So we'll stick with C and assembly to do this. Now, there are some other materials. And there's another one, the second reference, it's by a university teacher in Taiwan

who made a RISC-V operating system. We've also worked with it, and also, based on it, we studied it. We made our own improvements on it. Now, the third part is from MIT, a course. It's actually a course. If you're interested, and if you have time later on,

you can actually go and take a look at it. The one up here, the operating system developed in it, goes a step further than ours. It's actually basically getting very close to a system like UNIX. Now, for us, considering the time constraints,

we didn't cover it completely and go through it all for you. But we did use it as a reference. So if you feel that later on you have energy to spare, you can go take a look. It also includes some course materials, and the source code is provided in full as well.

That... Now, for other, more detailed reference materials, we've listed them separately in each chapter, so we've put together some lists for you, and you can go and look through them on your own Alright, so that's it for the first part, which is the course introduction

I'll just briefly stop here for you
