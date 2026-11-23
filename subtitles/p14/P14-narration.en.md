# English Narration Script

All right, next up is the final chapter on assembly instructions. This part mainly introduces the concept of mixing assembly and C programming, because it will come up in our code later, so we'll talk about it here. So mixing assembly and C really falls into two main categories,

one is calling C functions from assembly that's one and the other way around which is in C we can also embed calls to some or write some assembly code These are techniques you'll often use in low-level development. So let me walk you through them here.

Let's start with the first part, which is assembly calling C. This part is actually pretty simple, because in the exercise we just did this concept already showed up right? We've already touched on some of these related things.

Assembly calling C Now let's take a look at this together. That is to say, C doesn't really have anything special about it. C is, at the end of the day, just a high-level language. After it's compiled, it's really just assembly code and machine instructions.

So as long as we follow the function calling process we just talked about, the calling convention, we can actually write correct assembly-calling-C handling like this. Now here, the main thing is to follow the ABI, which is what we just mentioned.

That is to say, the ABI, which stands for Abstract Binary Interface, actually involves what we just talked about, for example, the size of data types. That is to say, whether it's assembly or C, you actually deal with data types,

for example, how many bytes an int takes up, how many bytes a char takes up, how many characters a character takes up— how many bytes it takes up, concepts like that. Then there are also some memory alignment issues, four-byte alignment,

and all that kind of thing. Actually, as long as it's cross-language, underneath it's all the same— you have to deal with all of it. Then there's also function calls, which we just covered. System calls— this might involve, mainly, the applications or user-mode programs,

and it's a concept about the interface between them and the operating system. Let's not go into that here. It's not really related to what we're discussing. Then there's another thing, when making a function call, there are certain conventions,

for example, the registers, we use a0 and a7 to pass parameters. The return values go in a0 and a1. These are all things we covered earlier. They become part of the specification. So for a specific example, let's take a quick look at one.

Very simple. This example, let's take a look at it. It's ASM to C. ASM stands for ASM stands for the abbreviation of 'assembly'. And 'to' m And this 2 here is "to" In English, it's just "to" As in from S-mode to C Right? That's what it means.

Just wanted to tell you all a bit about the naming here, why it's called this. So over here, we can see that down here, we actually have an assembly instruction, the test.s file, and right here, we have a test.c. So this is essentially mixed programming.

That is, what we have here is actually, we're... we start executing from the assembly in test.c first, and during execution, it will jump into this test.c So how does it jump? It's very simple. Let's take a look. First, let's look at this test.c.

This function isn't much different from what we talked about earlier. The main things to understand here are: One is that we first need to prepare a stack area. There's nothing much to say about this. Whether it's C or assembly, we're all writing functions.

And functions all need to use the stack. So we all need to prepare a stack area, and we need to take this stack and initialize it. We've covered this before. Then next, that is, we need to go from this start function Start executing.

In the course of the execution, we'll call a function foo. This function foo is just a function written in C. Now, as we mentioned earlier, a function name is actually the address of that function. Right? When we learned C, we also talked about the concept of function pointers.

The function name is the function's address. So when we're in assembly and want to call a C function, all we actually need to do is take that C function's name and write it right here. Right? It's really just a label. Just a label.

So when we call followed by a function name, it will naturally jump to that function and execute it. It's that simple. So the only thing we need to do, is that if we want to pass parameters, then here, we just need to use a0 and a1,

the two standard conventions, to pass the parameters, and that's it. Everything before this is the same. We pass 1 and 2 in. Now this function, it takes two parameters, a and b. So we pass the parameters, and then, and then let's pay attention here,

at this point, we need to take this function foo, We use the Global directive to declare it. Because this function is not defined in this file, you see? This is basically the same effect as extern in C. In other words, we tell the compiler

that this function F0 is defined elsewhere, that it's a global thing. Then when it compiles, it won't report errors again, saying it can't find this function, and so on. Because this function is defined in test.c, so we need to declare it here.

So basically, for the assembly part, that covers all the content involved. And as for the C part, there's really no difference in how it's written. Just write it the way it's supposed to be written. As for C, well, I think...

With this function we have now, we don't need to explain it much more. Now it's just about saying, when you all look at this function now, aren't you already thinking in your heads about the assembly language behind it, what kind of concept that is?

Once you've studied this, the way you look at C functions might have a certain... what do you call it? Do you call it a sublimation? Like, an elevation? Or some other concept? Meaning, the way you look at C now, actually, if you can... behind it...

the moment you see C, you can picture the assembly behind it, roughly what it looks like. Well, congratulations. That is to say, I think this course, at least you haven't learned it for nothing. Now, when you look at this C code,

this thing is actually just a label. Now, its two parameters, well, we can all imagine, when this code is handed over to the compiler to compile, when it generates the code for you, it will definitely help you put A0 and A1, which actually correspond to A and B.

At the same time, the return value C here, it will definitely be placed in A1, A0. This is something we definitely need to know. That's pretty much how it works. At the same time, when you look at this code, is it the case that the machine instructions generated from this code,

only this one instruction corresponds Now, what's the machine instruction corresponding to this one? No, it's not. We have a piece of code at the entry and that is, when we exit, that piece of code, right? This piece of code, after you compile it,

go and take a look, it's actually not the same. Now, can we take a quick look at it? Let me see, this one, let's take a look, can we see this piece of code? Let's take a look, say, right here, you can all take a look. This is what we see after disassembling it back.

Look at this part. This is that C function, our a plus b code, and the machine instructions it generates. Actually, in this part, the code before it, we actually correspond to... we didn't write any code to generate it at all. So where does it come from?

Well, the compiler does it for us. This is the epilogue. What is it actually doing for us? Pushing to the stack. See that? And then what does it do next? It saves some registers. And so on. And then, this code also won't be generated in our own code.

This code is also generated for us by the compiler. It's actually doing a store here. And then, it restores some data, and then stores again. And then return. Did everyone see that? Return, of course, corresponds to something similar to this here.

But then, why does it put the result here and save it into a0? That's also a convention. Because the return value has to go into a0. Remember that, right? So when you look at this assembly later on, as for why it's like this, I've already explained the reason to you.

This side is the second example. Now, this This is what we mean by assembly calling C. Assembly calling C. Very simple. Now for the second one, what we want to tell you is in C language, if we want to embed some assembly, how do we do it?

Just understand this. First of all, if you want to embed assembly in C, you need to write this kind of keyword, that is, a keyword starting with ASM. The syntax is like this. ASM is a keyword, and the square brackets after it mean

this keyword is optional. Then it's wrapped in parentheses, and inside are the assembly instructions, while outside it's C language code. Out here, everything is C language instructions. In here, in the middle, we embed a piece of assembly like this.

That's how you write it. Now, at the front, there are keywords. And then, volatile, it means that, if you add this, you are telling the compiler, these assembly instructions inside here, you don't want it to optimize them. If you do want optimization,

because the compiler might try to optimize your instructions, so if you don't want optimization, just add a volatile. If you don't care, just leave it out. So this part is optional. This sometimes you'll see in front of this there are some additional

double slash things like this which actually have the same effect I won't go into that in detail and then this is this is 'asm', which is the keyword and then inside it there are four main parts the first part is wrapped in quotes

enclosed and this part is the core of it all this is where the assembly instructions are written now, for the assembly instructions, they are wrapped in double quotes here and if you need multiple instructions, you separate them with a semicolon or backslash n

which represents a newline Just split it apart. This is its syntax. Then these three items below are all optional. Note carefully, they are just these three items, they're not actually assembly instructions. They're really a kind of

they're really a kind of directive statement, telling the assembler, for this assembly instruction in front of you, there are some special annotations. You can actually understand it that way. So what is it annotating? It's actually annotating

in your assembler, for this assembly instruction, what output operands and input operands there are. as well as what might affect this thing, let me briefly introduce each of them to you. Let me show you through an example. What's on the right is actually an example.

This example implements a we write a for function in C, and inside this function, what it actually does is implement an A plus B equals C, an effect like that. That is to say, if I don't use assembly, if this part is not written in assembly,

it's nothing more than implementing C equals A plus B. But suppose one day I suddenly feel that C plus A -- C's A plus B -- is not efficient enough, and I insist on writing it myself in assembly, then I can still rewrite and change it.

So how do we write it? Well, based on the description just now, you've already seen That is to say, this is a keyword that must be written Adding a volatile is basically telling our assembler that for this assembly instruction in here

I don't want you to do any optimization Just honestly follow whatever I wrote and generate exactly that Then, next, this here corresponds to this, the output operands. This part specifies and then this corresponds to the description of the input operands.

And the third part, I just didn't write it because there isn't really much that needs to be described there, since I only used two registers anyway. So, what do these output operands and input operands mean? Well, the so-called output operands

are essentially registers that I might write to— registers whose values I might change. Those registers count as output operands. That's the concept. Conversely, the input concept is the opposite. Now, what exactly am I explaining here?

Take a look at this. Because when I was writing this, first of all, I couldn't immediately— that is, I didn't want to absolutely specify exactly which register to use, because which register you actually use, in practice, we generally try to leave that to the compiler

to decide for us. We just tell it Here we need a register This needs a register This needs a register Then we take this register and we give it a name See that? We call it sum We call them add1 and add2 These are not actual register names

Right? What they actually turn into is something the compiler generates for us in practice We don't explicitly force that here The only thing we have to tell it is this What we need to tell it is this Since what we want to implement is C equals A plus B

we have to tell the compiler that you need to take this C and associate it with this register and you need to associate this register A with that You need to associate B with this thing Then we tell it through an add instruction

Just use this assembly instruction to achieve the effect, and that's it So how do we tell it About this correspondence between them? That is done through these two lines here Because as we said earlier This one specifies the output

That is, which register I'm going to write to So on my side here I tell it that the sum register corresponds to this c of ours Because in the end we have to return this c We have to pass it back out, right So we tell it c Then what about add1?

And what about add2? They correspond to a and b Which means we bind the registers with the variables defined in C We tie them together made a mapping telling the compiler how to carry out this mapping process so it's really a correspondence effect

it's really telling it the corresponding effect now, once that purpose is achieved the compiler will, in the actual process in the actual compilation process, assign us an actual register and then take this register and these things

and map them together and of course, on our side here you can see there's also this kind of R syntax written here this R syntax is really just telling it that is, we want to take this variable A and this variable C and map them to registers

you can also specify that you put this variable on the stack and map it to memory that works too you indicate that with the character M but here we'll use R just tell it to use the corresponding register, and that's all there is to it

So this is one way of writing it. Sometimes, you'll also see code that other people wrote, that is, written in a simpler way. If you want to keep it simple, you can write it like this. In fact, the effect is exactly the same.

Now here, all it amounts to is that we don't define any complicated names, like sum or anything like that, or that add1 thing from just now. See what I mean? What we actually used was what? We used 0, 1, and 2 to represent them.

Take this 0 here. It actually corresponds to what we just... This is a typo. The 0 actually corresponds to... our... This one is a typo. add1 is actually 1, and add2 is 2, right? An effect like this. Now, below, it's actually the same,

except that here it doesn't write 0 and 1, 1, 2 and those things, because by default it just follows this order. The first one that appears on its side, it uses it to mean 0. This one is 1, and this one is 2. Following this order,

they can just be specified by default. So with this kind of thing, is a simplified form of writing. So once you understand what it actually means under the hood, you'll see that in a lot of code, what's commonly used is still this simplified way of writing it,

and you should be able to read it. So this gives us a way to write some assembly in C. Of course, the example here is quite simple, and there may be more complex syntax, for instance, since you can write multiple instructions and so on.

For more details, you can go to Reference 3, an assembly manual, which has a detailed description of how to embed assembly. We won't go into that here. Just be aware of it. Then, when you get back, try doing some exercises on your own.

It's this 5-9. So basically, what it means is you need to take this thing and convert it into assembly. See how you would write it. Give it a try on your own. Now, regarding this, when we talked about assembly instructions, we spent quite a lot of time covering them with you.

It took quite a bit of time. There's actually quite a lot of content. We still covered some key points, as well as things that may come up in the later parts of our course. We touched on those here. As for the rest of the content,

it will probably still be up to you, during the exercises that come later, to learn it on your own.
