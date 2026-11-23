# English Narration Script

We still have some time this morning, let's refresh our memory. We've only covered the first category of instructions so far. Now, the second commonly used category is logic operation instructions. As for logic operations, they actually show up in C as well — that's AND, OR, NOT, essentially AND, OR, NOT.

Because in practice we've found that, apart from operations like addition and subtraction, these AND, OR, NOT operations are actually quite useful when programming. I'm not sure if any of you, in your programming experience so far,

these AND, OR, NOT operations, in what scenarios have you used them? Does anything come to mind? Probably in high-level languages, these scenarios don't come up very often. But we can't rule out that languages like Java do provide these operations.

However, in the exercises that follow, including low-level development, for this kind of work, since we often operate on binary data, when we manipulate binary data directly, then these AND, OR, NOT operations they are still used very frequently

so you should still understand them so what about AND, OR, and NOT? we should note that in RISC-V please pay attention here in the RISC-V instruction set it provides AND, which is the AND operation and OR, the OR operation but it does not directly provide

the NOT operation that is, in C-like terms the exclamation mark operation bitwise negation it does not directly provide that operation but it provides another operation instead namely XOR XOR that is, XOR please note this the point is, this XOR operation

We actually translate it into Chinese as 'load-reserved', or LR. As for LR, do you still remember which symbol we use for it in C? It's that arrow pointing up, right? I think so. Isn't it? That is to say, LR is directly provided

by the base instructions. Now, as for the concept of SC, let's take a quick look at it here. What does it mean? SC in English, it's actually 'exclusive'. 'OR' means 'or'. It means 'or', which is the store operation. What 'exclusive' actually expresses,

is an exclusive store. It's also a kind of store operation, but it's an exclusive store. So what do we mean by an exclusive operation? Let me explain that for you later. That is, when two Because all of these logic operations work bit by bit.

In other words, if the two bit values are different, that is 1, right? Different. Then the result is 0. So what you get is the effect of OR. As we know, OR means that when the two are the same, the result becomes 1. Isn't that right?

So this one works the other way around. It takes two bit values, and if they are different, it sets the result to 1. If the two bit values are the same, the result is 0. That's the concept of XOR. This is something you need to understand first.

Now, some of you might ask, that is to say, for the NOT operation, how do we implement it? Well, in RISC-V, the NOT operation is actually implemented through XOR. This is its instruction format. It doesn't provide a new instruction format.

As for this NOT operation, RISC-V actually provides a pseudo-instruction called NOT. This NOT instruction as an instruction, is actually equivalent to what? It's equivalent to this operation equivalent to this Think about it, why does it achieve this effect?

Its actual effect what it actually means is, take a number take a take a take the number in a register, XOR it with -1, and put the result into rd — that is, negate it. Now think about why. Let's take a look. Let's give an example.

For instance, Let me give a simple example. Say we have an original number that is 10101010. Suppose we originally have a number like this. This number, we might initially put it in rs here. Now, what is this -1 actually? How is -1 represented in a computer?

It's all ones. Right? It's all ones. So, so what this thing does, is it takes these two numbers, this is -1, this is negative one, and performs an XOR operation on the two numbers. Now, what result do we get from this XOR operation?

So what did our definition just say? XOR means that if the two bit values are different, you get 1, and if they're the same, you get 0. Right? Now look at what happens here. 0 and 1 — we know that 1 has this property: that is, what's the characteristic of 1?

Well, for any number, if it's 0, 0 and 1 are definitely not the same. Since they're not the same, you take 1. If they were the same, you'd take 0. So you see, this is basically flipped, right? Right? This one, I'm not going to do it one by one.

Basically, it has to do with this: all of these 0 and 1 bits, when XORed with 1, actually produce an effect where It just gives you the inverted result. So we actually don't need a dedicated instruction just to do negation. We only need to use the XOR instruction

to replace it, and that's it. This once again shows how RISC-V is all about saving wherever it can. When designing instructions, it won't add a whole instruction just for one function. If an existing instruction can do the job,

it just uses the existing instruction instead. And the benefit for us is that a lot of the time, we only have to master a very small set of instructions. But on the other hand, it may ask more of you, and that is this: That is to say, at the very least,

you need to have some understanding of this instruction, otherwise you won't know that you should use this instruction to replace it. Right, everyone? So this reflects a certain philosophy in the RISC-V design. These are what we call logical instructions.

This is XORI here, and of course, over here it's ORI. Just so you're aware of these. As for the related exercises, you can take a look at them after class. These two exercises, we won't go over them in class either. So what we just covered was logical operations.

Now, when we're processing binary data, another thing we use very often is the shift operation, shifting left and shifting right. This kind of thing is also used a lot in C, right? Let's all pay attention here. That is to say, in RISC-V, its machine instructions — the shift operations it provides fall into two major categories.

One category is called logical shift. The other is called arithmetic shift. Let's first take a look at what a logical shift is. So for logical shifts, we can see they're divided into two groups. The way I've written it here, it's actually split into two groups.

One group has an 'i' appended to it, which means that instruction will actually carry an immediate value. The other one has no immediate value, and operates using three registers — that is, the value to be shifted Let's take a look first at

what logical shift means. So for logical shifts, you can see they're divided into two groups. The way I've written it here splits them into two groups. One group has an "i" attached, which means that in these instructions there is actually an immediate value

while the other group has no immediate value. It operates with three registers. That is, the shift amount is expressed by putting it into this register. And this one, actually specifies the number of bits directly through the immediate.

Now, it's divided into left shift and right shift. So, logically speaking, it's divided into two types: left shift and right shift. The left shift is 'left' and this side is 'right' which is the right shift. Actually, these are just abbreviations of these letters,

like 'shift left logical' which means logical left shift. It's completely reversed compared to how we Chinese say it. Foreigners' word order is totally different from ours. Foreigners' word order, our word order is completely different from theirs.

We Chinese flip it and say 'logical left shift'. Foreigners say 'shift left logical'. It's completely reversed. So, this one But we still need to get used to how it's used, otherwise you won't remember this command. SLL is a bit of a tongue twister,

this SLL command. Why is it called a logical left shift? That's where the name comes from. Now the principle is very simple. It's just left shift and right shift, taking that data and shifting it left or right bit by bit. But here's something to pay attention to.

A logical shift has one characteristic, and that is, as you know, whether you're shifting left or shifting right, when you shift a piece of data over, say, to the left, the position on the right becomes empty, and new data has to be filled in.

Think about it — isn't that right? If we shift to the right, the high bits become empty, so we have to fill in data for those high bits. Now, for a logical shift, the rule is that the filling is always done with 0s. What does that mean?

Let's take a simple example. That is, suppose we now have... let's say we have 8 bits. Because the number is too long, it might look a bit messy. Suppose we have this data now, 000100. Suppose we now want to shift it to the right.

Let's say I now take this 1 and shift it to the right. So shifting it to the right, because as we know, So where does this 1 go? It ends up right here. Think about it — right? Right here. So what about the 0s on the right here?

They get pushed out. Nothing much to say about that. So what's left here? There were originally 4 zeros, and now there are only 3 zeros. Now over here, these 3 zeros over here, naturally follow this 1, and shift to the right. So over here,

it's of course 3 zeros. And here, one bit gets freed up, because after it shifts right, what we're talking about right now is a right shift, a shift to the right. Shifting to the right. So with a right shift, this side needs to be filled in.

That filling in is exactly this concept. So with a logical shift, note that when filling in, everything is filled with 0. That's what it means. This is called a logical shift. And shifting left works the same way. This is called a logical shift.

We use logical shifts all the time. I wonder if any of you have used, say, C to program, you'll actually still use this kind of thing. So what do we often use these shifts for? Have you thought about some scenarios? For example, we often need to

let's take a simple example. Say I have two 8-bit numbers now. Now I want to piece it together into a 16-bit number What do we do? Well, maybe we take the high part of that, well, maybe we take the high part of that, Since we need to build that high-order number,

we have to shift the low-order number to the left, shift it left by 8 bits, and it becomes the 8 bits of the high part, right? Think about it, isn't that right? This kind of thing is a left shift. We often right shift, and we use it in this kind of scenario.

This is logical shift. Now, opposite to that, there is something called arithmetic shift. Arithmetic shift and logical shift are somewhat different. First of all, arithmetic shift only has arithmetic right shift, there is no arithmetic left shift.

Please note that. Also, what exactly is arithmetic shift? Arithmetic shift means that unlike a logical shift, which just fills everything with 0 regardless, it operates based on your sign bit. Let me give you an example. Say, for instance, at this position from just now,

this 1, for this number, we know where its sign bit is. The sign bit is usually just the most significant bit. Suppose it is 0. OK. Then if it's a, just like the arithmetic right shift we saw earlier, if the most significant bit is 0,

then it will pad with 0. But suppose what we have here is not, not 0, and suppose there is a 1 here instead. So think about what effect that produces. What gets filled in here is no longer 0, but what? It's 1. So actually, after the right shift,

you end up with something like this. This 1, this 1, this 1 actually got shifted over from this side. Everyone with me? But how did this 1 get in here? This is exactly what arithmetic right shift shifts in. Because during an arithmetic right shift,

it sees that you originally had a 1 over here, so what I shift in is a 1. Follow what I mean? If this side were 0, then this side would become 0. That is arithmetic right shift. It actually comes down to the idea of a sign bit.

That is to say, with arithmetic right shift, it fills in the bits based on your sign bit. Now, some of you might ask, why do we say here there is only arithmetic... with arithmetic, why is there only right shift and no left shift?

Just now, that logical shift, as you can see, had both left shift and right shift. Logical shift. Arithmetic right shift. Why is there no left shift? Think it through. These things are all very simple. It's really not that hard.

Think about it. What effect would a left shift have? It would shift your most significant bit right out. Get what I mean? If the highest bit gets shifted away, then as far as it's concerned, it loses that meaning. The sign bit is gone.

Get what I mean? So for arithmetic operations, we would never do a right shift or a left shift, Pay attention here. This is the difference between arithmetic shift and logical shift. Pay attention here. So what can arithmetic shift be used for?

Well, with arithmetic shift, for example, let me give you an example. Say I have a number called negative 4, and I want to divide it by 2. What method can we use to do that? Actually, we could use the dumbest way, which is to use the division instruction.

Because we haven't covered the multiplication instruction here. We do have the trigger instruction See what I mean? There's the DIV trigger instruction We could use the trigger instruction to implement it, of course But we can actually implement it with arithmetic shift

Because as we said, shifting to the right is actually equivalent to division Why? Let me give you an example Like we just said, let's keep it short, suppose this is a number, 4, let's say, the number 4, if we shift it right by one bit,

what does it become? It becomes 2. See that? Hasn't it just been divided by 2? Now, similarly, If there's a sign bit, it will still reach the same so for negative four, it becomes negative two. Now, logical left shift, logical left shift,

actually, I forgot to mention this earlier, logical left shift, can actually also be used as a kind of multiplication. Left shift is actually equivalent to multiplication. However, with this logical left shift, when doing multiplication,

everyone should note that it can actually only support multiplication of positive numbers. After left shifting a negative number, its sign bit gets shifted out, and it's lost. Think about it, right? So actually, when implementing this complex multiplication,

you can't do it purely through logic operations. Keep that in mind. And also, using this shifting approach, compared to using dedicated multiply and divide instructions, it's actually more efficient. If you have a lot of these shifting operations,

using shifts is actually better. You can write your own C code for multiplication and division, then use GCC to generate the machine instructions, and you'll see that the compiler actually performs optimizations. It will use it will try to use shift instructions instead of these

multiply and divide instructions. Keep that in mind. So that's the shift instructions. They're used quite a lot. Now, for the related exercises for this, I'll just list them here. You can work through them on your own when you get back.

In class, if we did it this way, doing one and writing one, if we did one and wrote one up, I reckon it would probably, take a lot of class sessions just to get through this part. Because assembly is actually, a course in its own right,

offered as a dedicated class. What we're really doing here is, as part of RISC-V, giving you a quick run-through. So time is still pretty tight. Now, this exercise 5.4, is also meant as homework practice. It's the one I've highlighted here.

If it's the practice exercises, the practice part, we actually don't provide the source code. So treat those as exercises to do at home. Basically, after we finish a chapter, the related exercises, you can all go back and work on them.

Now let me see how much time is left. I'll probably finish the load and store instructions. That's about all we can cover today. So besides what we just talked about, this addition, subtraction, addition, subtraction, and then logical operations,

AND, OR, NOT, left shift, right shift, arithmetic and logical, besides these shift operations, we also have another operation that's performed frequently, and that's reading and writing to memory. As we mentioned earlier, all the ones like addition, subtraction, multiplication, division,

the shift instructions It can only operate on registers directly. But what's in the registers, in general, we always need to read it in from memory. Now, the concept of reading, is called a load. Note this carefully, this English terminology,

load and store. Load actually means bringing something in. It's loading something in. From your perspective, you're looking at it from the CPU's point of view. Do you understand what I mean? Store, on the other hand, means storing something out.

It's the concept of sending something out. So it's a concept of writing. I've marked this with colors here. When you look at this diagram, try to understand these concepts. To make it easier for everyone to remember this instruction,

the abbreviation, is L and store. L and S. Now, one thing to note, that is, taking load as an example, for reading, and writing is the same, there are actually some variations. As you can see, it can it can read byte by byte, or it can

read two bytes at a time. That is, the meaning of B, H, and W here, which stand for byte, half word, and word. We need to have this concept. byte is a byte and word is four bytes half word is half of four bytes right? So that's two bytes.

And the abbreviations are B, H, and W. Just understand it this way. This is L. This is divided into three groups. At the same time, for these three groups, there's also something else, that is, look here, there are some variations.

For example, there's a U here. So let's take this one as an example. Let's distinguish this. What's the idea behind this U versus not having a U? I mean, if it's... What's the concept here? What issue comes up when you want to read the value in?

So we know how wide our internal registers are, right? Our internal registers are 32 bits. Right? But now, take LB for example. When you do the read at this point, how much data are you reading? You're reading one byte. That is to say, your register is four bytes wide.

But what you read is just one byte. So here's the issue. Once you read that one byte in, in that register of yours, what do you put in the remaining three bytes? You see what I mean? For example, this register here, it's four bytes wide.

What we read in is only one byte. Now, that byte, we generally put it in the low bits, and that's pretty much it. But now there's one issue, which is what to put in the upper three bytes of the register, the question of what goes there.

See what I mean? There are two cases here. One is called sign extension, and the other is called zero extension. We covered these concepts earlier. What is sign extension? It means that if the byte you read in here, this byte, has a most significant bit of 1,

then what does sign extension mean? All the upper bits become 1s, right? Still following me? Extend according to the sign bit. If it's 0, then everything is 0. This is called sign extension. But if you add a 'u' here, then it becomes

actually unsigned extension. Unsigned extension means zero extension. Zero extension means that no matter what the preceding byte's most significant bit is, 1 or 0, the upper bits are all cleared to zero. That's the effect we achieve.

This actually maps to our C language in a way that is basically like somewhat similar to the concept of unsigned and signed numbers. Take a moment to let that sink in. And then, when it performs the operation, note that it works like this:

that is to say, It goes by this rs1 the value in this register its value that is to say, since you are going to memory to read then this memory of yours, this address in memory, you definitely have to tell the computer. So how is this memory address computed?

It is obtained by rs1 plus an immediate value. Because it uses the I-type format. You can imagine that since it is I-type, in this instruction, one part holds a 12-bit immediate value. This immediate value represents this offset here.

So it actually takes the value in this register, adds this immediate value, and finally gets an address. Then, based on this address, it goes to memory to fetch that byte. That is actually how the process works. So, everyone, this shouldn't give you too much trouble.

Now, when we talk about offset addressing, I've listed a few examples here. Take a moment to get familiar with them. And at the same time, also, notice this address range. Why is it this one? It's also because of this immediate value,

which is 12 bits wide. There's one more thing I want you to pay attention to, and that is, there's a question here, let me ask you, I want to ask everyone, just look at this, LBU, LBU, LHU, LHU. But then, why isn't there something like an LWU?

Think about why. This is actually something we covered earlier, based on what we explained to you before, about why we do sign extension and zero extension. I think everyone should get what I mean. A register is four bytes, and if you load a word,

what does that mean? A word is also four bytes, so the data in memory, when loaded in, exactly fills up one register, right? Think about it, isn't that the case? Do we still need to extend? No extension needed, naturally. Right? So these

As you study, ask yourself these questions — a few whys. It may give us a deeper understanding of the design of this instruction set. That is to say, for LW, there is no LWU, and the reason is that sign extension isn't needed. So this example,

you can go back and match it up yourselves, and see the effect of this sign extension. Now for writes, it's relatively simple. For writes, there are only B, H, and W, with no sign extension. There's no distinction between unsigned writes and signed writes —

store instructions don't distinguish them. Now think about why that is. This is also something worth understanding — why. Everyone think about why. Because this is actually the reverse of the process we just talked about. You know this.

For example, take a byte — one byte. Think about it. Inside the register there are 4 bytes. Outside there is only 1 byte. Now you want to put this thing out. This is not reading it in. Do you understand what I mean? Since you're putting it out,

the CPU will naturally just fill in its low-order byte and that's it. There's no such thing as a concept of extension here at all. Do you understand what I mean? So naturally, there is no extension issue. So when writing out,

you don't need to worry about this sign extension issue. So when you write instructions, if you know the reason behind this, it may help you understand it better. Now, its instruction format is a new instruction format called S-type.

The S here stands for store — it just means store. That's where the name comes from. Now, some of you might wonder, why does this immediate look so odd? Look at this immediate — when it's actually stored in the instruction, it's split apart, as you can see.

5 bits are placed here, and here you have bits 5 to 11, which is 7 bits. Why not just put these two parts together? Well, honestly, when they designed the spec, there must have been a reason. And that reason probably has little to do with software.

It's entirely because, possibly, at the lower level... From a hardware design perspective, it comes down to a decoding efficiency issue. As for the reason, we don't need to worry about it. When we write, when we write assembly,

we actually don't pay attention to the specific arrangement inside this. But when decoding, when you disassemble, you might need to consider this issue. So, as for memory reads and writes, we'll stop here for today. Then there are still some conditional branches

and jump instructions. We probably won't have time to cover those today. We'll cover them in the next class. As for the assembly instruction part of the content, you'll notice that compared to the earlier material, it's definitely a bit more dry.

Because especially when doing the exercises, It's also a bit different from writing in a high-level language. But because in this course of ours, in this course, in the end we'll be writing some cutting-edge stuff, low-level development.

That means at the very least you need to be able to read it. You get what I mean, right? If I give you a piece of assembly, you should at least be able to roughly understand it, and then make some small modifications. That's probably about all we'll ask of you.

Writing large blocks of it, you won't need to do. But, as long as you have the basic concepts down, and can read it, that's fine. OK. So that's it for today's class. All right? Thank you, everyone.
