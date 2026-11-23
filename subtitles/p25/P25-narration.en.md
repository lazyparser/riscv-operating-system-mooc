# English Narration Script

Next, we will introduce Chapter 15. The topic of Chapter 15 is software timers. This chapter is relatively short and is mainly divided into three parts. The first part introduces the classification of software timers. The second part introduces the design and implementation of software timers,

and the third part introduces that is, when it comes to software timers, there are some optimization scenarios Let's first look at the first part: classification. So, timers, in our previous chapters have actually been introduced to you already.

There are hardware timers and software timers. In one of our earlier chapters, we actually talked about hardware timers. In Chapter 12, as for hardware timers, from our earlier study, we know that for hardware timers, their advantage,

which was also mentioned here, is that because they directly use an external interrupt to send notifications So the advantage is high precision Right For example, that external timer of yours If you set its precision to 10 nanoseconds

Then when you actually run it It can guarantee that at 10 nanoseconds At that fixed frequency It can deliver interrupts to you So we can actually use those 10 nanoseconds as our precision To carry out some logical control However,

Its drawback Is of course also quite obvious Which is that up there there's only this one Right A limited number You can't just have as many as you want Logically speaking, if When we normally develop applications at the upper level

It's possible that if When it comes to creating timers, I don't know if any of you have written this kind of program, meaning you might create them multiple times, or you run a lot of timers. Now, if the hardware can only handle one,

and you rely entirely on the hardware timer to do the timing, then that's simply impossible. So, on top of the hardware timer, we need to, essentially, inside the operating system, offer you a richer set of timer services, implemented in software.

And that is what we're going to introduce today: the software timer. Now, the software timer, as we say, it is based on, it is actually built on the capabilities provided by the hardware timer, wrapped up and layered on top of it,

to produce the final result. It extends the limits of this hardware timer. It can That is to say, since it's implemented in software, think about it — isn't it true that then, that is to say, as long as our in other words, you can implement as many as you want,

as long as your code this memory or whatever there's no such limit, then we can provide more timers. But the downside is that it's relatively low, because we know underneath it's still built on the precision of the hardware timer.

So your precision cannot exceed that of the hardware timer. And on top of that, once you stack some software implementation on it, its precision will in practice generally be lower than the precision of the hardware timer. But,

and moreover, As for its precision, because this concept of TIC we already covered when we talked about the hardware timer, right? So, when it comes to this, its precision, in general, is also an integer multiple of TIC, since underneath, it works in units of TIC

the hardware timer triggers in units of TIC. So it's triggered in those units. So on the layer above, in general, its precision can only be an integer multiple of TIC. That was the broad classification of timers we just mentioned.

Now let's focus on software timers. As for software timers, in theory, we can divide them into several types. First, if we go by how the timer is set, we have two types. One is called one-shot One is called periodic What do we mean by one-shot?

Well, it's basically like saying the timer interrupt we set up earlier is essentially one-shot in nature Meaning, every time the timer fires that timer naturally becomes invalid Right? If you still want to achieve the periodic behavior I'll talk about later

then when we implemented this periodic triggering earlier actually, every time the timer fired we had to set it up again Right? Only by setting it up again does it basically trigger once more So since each time is really still one-shot

and the idea of periodic means that it's basically like saying you only need to set it up once After that, it will keep triggering automatically. That is the effect we want to achieve. This is called periodic. Now, with this kind of periodic behavior,

usually we can only wrap it up through software. So a hardware timer, in fact, can only be one-shot. But with a software timer, we can wrap it up to be periodic. In other words, you only need to create it once, and from then on it keeps firing periodically, infinitely.

Unless you explicitly stop it, this is periodic firing. Now, if it follows this execution model, that is, when the timer expires, we know it must go and execute a timer handler function, a response handler function. Now, this handler function,

the context and environment in which it executes may vary. And depending on the different execution contexts and environments, We divide them into two kinds. One kind is it executes in the interrupt context environment. And the other kind

executes in the task context environment. Now, what does it mean to execute in the interrupt context environment? Take our earlier hardware timer example, which is definitely executing in the hardware context— that is, the interrupt context environment.

So if it executes in the interrupt context environment, then this kind is somewhat similar to the previous spinlock in terms of its effect, maybe a bit alike. That is to say, it's the same, because it runs in the interrupt, so when it executes,

the execution process It has to be as short as possible. It can't be too long. Otherwise, it will cause the response of the overall system to slow down. But the right side, is also quite clear. That is, it definitely responds quite quickly,

right? The real-time responsiveness is relatively high. So this, if it is so-called running in a foreground environment, actually that means because essentially this timer mechanism is all triggered through interrupts. It's just that

If you want to for example, if you have a handler that takes a relatively long time and might even go to sleep then how do you achieve this kind of this kind of effect? Actually, the only way is that inside the interrupt handler

you spawn a new task or make use of an existing task and use that task to carry out your actual logic So as far as this interrupt is concerned, it itself still executes quickly and returns but because your actual work is deferred to a task's context,

implemented in a task's context, then in this situation, we consider this kind of timer handling to be a task-context processing environment So its advantage is that because it is implemented inside a task, the task can be suspended

or put to sleep. But the real-time responsiveness is definitely a bit worse, because the code execution inside the task depends on when the task gets scheduled. And scheduling time doesn't mean that once your timer expires, you will definitely be scheduled.

It might only get scheduled under other conditions. So its real-time response might be relatively a bit worse. So for everyone today, in today's class we implement the timer, relatively speaking, not making it that complex. Actually, the main focus is to introduce the one-shot timer,

and the function we execute is also implemented in the interrupt context, which is relatively simpler. Alright, so now let's look at this class, where we implement a software timer ourselves, and some of the considerations involved.

So first of all, as a piece of software, when we build this software timer, it's really just about as I just said, it's really just about providing a better, for the application layer, providing a better service, right? So externally,

the interface we provide, we need to nail that down first. And this interface, it's fairly simple. First of all, on our side, we've defined a struct, and the relevant code, you can go and look at that. code os10 sw is short for software

the software timer's header file inside the os.h header file defines the related interfaces and functions Now, this timer is actually a struct for our software timer Inside this struct, our design is relatively simple The first two parameters

are both related to the timer's execution function When a timer expires, all it does is execute a function And that function is of course defined by the user After defining it, we register it into the kernel and the kernel saves it

When saving it, Here, it's essentially saving the entry address of the function, which is a function pointer. And this is a pointer to its parameter. This parameter will ultimately be passed inside, so when this function is called back internally,

it will still be passed in. Those are the first two parameters, the first two attributes. Now, the third attribute, is actually related to our internal software timer implementation, a timeout counter. As for exactly how to use this,

we'll explain it to you later. So this is one of its internal structs. Now, below this, are the two interfaces provided. One is for creation, and the other is a deletion interface. Creating one means, for example, if the user wants to

create a timer themselves, they just call this interface. Then when it's no longer needed, you just delete it. Now, when creating this interface, the parameters it passes include the two parameters you see here, which actually correspond to this thing.

That is, after the user defines a function outside, they pass in its address and parameters, and we'll record it. Then, this timeout value, basically means, for a timer, you want to go from, say, the current moment, for how long,

which is the timeout. this is when the timer fires that is, it is specified through this and as for this one, its unit is the tick let me just mention this briefly because what we built is fairly simple, all of our internal timing

is done in units of ticks now, the concept of the tick when we talked about the hardware timer we already covered that if you don't remember, go back and review it and as for deletion, after this function executes successfully,

it returns this the pointer to the object that created this timer so when deleting it, you just pass this pointer back in again We can then find the corresponding struct, and stop it. This is the set of interfaces we designed. Now, inside the kernel,

internally, we actually use the simplest array-based approach to implement it. For now, that's how it is. An array approach. We define a software timer array, and it can hold at most 10 timers. This way, of course, that number could be larger,

but this is just for demonstration, so we set it to 10. Actually, you could open as many as you want. And then there is also the timer's This is the initialization function for the entire module. This one is generally not meant to be called directly by users.

This is during our kernel boot process, it goes and initializes the whole software timer -- essentially a subsystem -- Now, internally it's very simple. It's really just a for loop, because it's an array, so we iterate through it once,

and then we take every member inside and reset it to an initial -- reset it to an initial value. So mainly, it sets its callback function and parameters both to NULL. And then, its create function and delete function, let me briefly introduce each of them to you.

So the create function, it's actually also very simple. So this preceding part of the logic, It's basically looking for an available one. Since it's an array, it goes and looks. When it searches, the check is done through this:

whether the pointer to its callback function is null or not, to determine whether this timer has already been used. This array entry, whether this member is being used. So as you can imagine, when deleting, we also need to make sure to take this

and set this field of the member back to null, and that's it. So once we find one, we break out of the loop. If we do find one and come out, then after coming out of this, Take the parameters here Take the parameters passed in by this system call

and place them in one by one to save them Then here there is a timeout value We will take this take this timeout value based on this variable When we talked earlier about the hardware timer you may remember it is actually an internal

global tick count Every time the timer fires that is, when the hardware timer fires this number increments by one So we will use this to which is basically saying It keeps track of the system's current tick count. Then we take that tick count

and, based on the timeout passed in, add it together, and save it into this member. So basically, what this does is record the number it will reach when it expires. Later on, we'll keep comparing this number against the tick count.

When they are equal, that means the timer has expired. It's really that simple. So that's the logic behind creating one. Now, what about deletion? That's also very simple. As I mentioned earlier, through the pointer to the timer we pass in,

we find the corresponding entry in the array. Once we find it, we simply clear it out. and that's all there is to it. As for this timeout field, we actually don't need to clear it. Because every time we create a new one, we assign it a new value again.

So we don't clear it. For the other parts, we just make sure to clear them. Especially this one, it must be cleared. Because we use it to determine whether this timer is, by this, this, this array member, whether it is currently in use — that's what this flag is for.

All of this is pretty simple. Now, as for the execution logic, we use the hardware timer underneath to actually drive the logic of this software timer. So the overall execution flow, roughly speaking, is like this. Let me quickly walk you through it.

That is to say, For example, once an interrupt occurs, the hardware that is, once a timer interrupt occurs, then based on what we introduced to you earlier, we know that this piece of logic actually hasn't been changed much. It definitely enters our

interrupt handler, the exception handler trap. Now, the trap, based on our judgment, it is actually a... we know this one should be classified as an interrupt, right? It's an interrupt. Now, for interrupts, we say there are several kinds:

there are software interrupts, timer interrupts, and external interrupts. Right? Let's recall. Then 7 is the timer interrupt. For the timer interrupt, we enter our time handler, that is, the timer interrupt handler function. So it goes into this function of ours.

Now, in this function, the logic at the beginning, we actually mainly in the logic we had at the time, there were mainly these three steps. Right? This one is the maintenance of the global tick count. This one, no, sorry, that was wrong.

This one, this one loads the hardware timer for the next time. This is just a quick review. Now the third one is related to scheduling. Because this is essentially the core logic of preemption. So here, in our case, we mainly added this function called timecheck.

This is the logic we added for our software timer. When you look at it, compare the two side by side. Now, timecheck, that is the core logic behind our software timer implementation. So inside this function, it actually uses this to go through this array,

based on the current tick count, the global tick count maintained by the hardware timer, and the value stored inside each timer. We talked about this value earlier. It actually records when this timer is supposed to expire. It checks whether this tick count has become greater than or equal to it.

If so, we consider the timer to have expired. Once the timer has expired, we use the recorded function, this pointer here, and we go ahead and call this software timer's handler function. This one is set by the user. So inside here, we basically do a callback.

We call back our timer handler function, the software timer handler function. And after the processing is done, we just clear out this timer. So what we've implemented is only a non-periodic, one-time, single-shot timer. Also, please note something.

Since this whole process runs in interrupt context, the execution of our function here, in fact, belongs to this kind of interrupt context. Relate this to the concepts we covered earlier. Our current implementation uses this approach.

So when you implement this, when you use our software timer to do the software timer handling, there are actually some issues to watch out for in this function. Because it runs in interrupt context, you need to pay attention to what we already covered earlier.

Now, after this call, the whole process... So this is the trigger process of our software timer. Earlier we talked about creation and deletion. As for triggering, it goes through this path here. It mainly involves the timer interrupt we mentioned earlier,

this concept. Now let's go through a few simple examples. Let's see how this timer works, and how it meets our requirements. So this here is a diagram. The tick on this side, that's the one we talked about earlier, maintained by the internal hardware timer,

the global tick count, the system tick count. And over here is the list that implements our software timer. Right now it's actually an array. So let's say this here is our timeline. Let's say our current time starts from a tick count of 100.

As for the tick, let's say in our code, the tick period is set to one second. That is, a hardware timer interrupt comes once every second timer interrupt hardware timer interrupt once every second So suppose at the moment of tick 100,

we create two that is, we call timer_create, creating two software timers Now suppose what we want is for the first software timer to time out after one second, and the second software timer to time out after two seconds Now the key question is,

for our setting, what should its timeout value be? Well, you can imagine it like this Suppose we're the first timer, to time out after one second then it times out at this point and the second one after two seconds It's this point that times out.

So it's nothing more than a 101 and a 102, right? Now, when we actually set it, the parameter we actually pass in, really just needs to be 1 and 2. But since we add 1 and 100 together, we get 101. And for 2 and 100, adding 100 gives us 102.

So the values we actually store in the internal software timer, the member values, are definitely a 101 and a 102. Alright, so suppose this is all set up at this point, and we start running. Now this interrupt, it keeps firing.

And the Tick count, every time a hardware interrupt comes in, this value keeps changing. And when it reaches 100, Based on the logic we talked about earlier, every time the timer period comes around, besides incrementing this value,

it goes and does a timer check. Now, for the timer check, it will go and compare, iterating through all these timers, comparing their timer values against this tick value. Once this value is greater than or equal to it, and the condition is met,

that means it has expired. So at this point, this Time1 on the left has expired. Once it expires, it has timed out, and the timeout will trigger its handler function. So from the software level, this timer has timed out. And after the timeout is handled,

it will be deleted. So next, the second one. Now suppose at this point, at this point here, we get another timer, timer3. So for this timer3, let's say it's again a three-second timeout. Now think about it, what should this value be

at this point? Well, based on our earlier experience, as you know, since it's a three-second timeout, that means, starting from this moment, it times out after three seconds. One second, two seconds, three seconds. So we definitely need to put 4

right? But when we pass in the parameter, we're actually still passing 3. So how do we get 104? Very simple. Because this value, right now we're adding this value to 3, think about it, isn't that right? So naturally we get 104. So the way we do it here,

what we actually put here is 104. Now, the three of them are created. It was created at time 101, and it expires after 3 seconds. Now suppose we reach 102 at this point, after 102, as you know, this timer expires again, so the second timer times out.

triggers the corresponding function. Then, next, the tick count goes up by 103. At 103, it actually hasn't timed out yet. No timeout. Then, when it reaches 104, as you know, time3 will also have timed out by then. So from this example,

we can see that, even though there is only one hardware timer underneath, on the software side, we can actually set up, set up many timers at the same time. So how do we implement that? It's actually done using the mechanism we covered earlier.

This is what's called a software timer. Take a moment to let that sink in. Now for the third part, basically, the software timer we implemented earlier, is fairly simple. In fact, this software timer There are many places where we can optimize.

The main goal of optimization is this: we want the software timer to handle things efficiently. Because as everyone knows, with timers, if you set up a lot of timers, and you traverse them using this kind of array-based approach,

the efficiency is really not going to be very high. Don't you all agree? Especially since every time we have to go in here, and if its timers are implemented using an array, the memory has to be allocated upfront. So the memory usage is not really optimal either.

So there are actually many places where it's worth optimizing the implementation of this software timer. Now, this is the current implementation of our software timer. We can see that because our core data structure is designed as an array,

a lot of the code in here relies on a for loop to traverse it. And this array also reserves a lot of space. So as you know, if you were asked to implement this kind of thing, the first thing you'd probably think of is, first, from the perspective of saving memory,

Or, in terms of flexibility, we actually prefer to use a linked list. Think about it, right? So with a linked list, the first thing is that the allocation of the data space in memory becomes much more flexible. You only create as much as you need,

rather than like our array, where we allocate a big chunk all at once. Because with software timers, there might be cases where we need a lot of them, and then the pressure on our array would be quite high. So that's why we use a linked list approach.

Now, there is another thing, which is that, even if we do use an array, as it stands right now, when we actually go and create these objects, we simply check whether their space is empty. Now, during the search process, with the timers being used,

more and more, that is, once some of them are used up, they get cleared out. So there are a lot of holes in the middle, right? So this is actually not very efficient. And besides, our time ordering — it's possible that, for the timers in the array, with longer timeouts,

they end up being placed at the front. So actually, if we, sort them by their timeout time, that is, keeping this guaranteed, inside the linked list — whether it's a linked list or an array — if it's this, its timer's Timeout value —

if it's sorted by time, So the probability of us hitting it, actually, since timers must expire in chronological order, that's one of its characteristics, that is, the closer a timer is to the present, the more certainly it will be placed at the very front of the list.

That will certainly speed up the odds of hitting it, so it may be a bit greater. So in fact, we have many kinds of optimizations. That is, the first one is, sort the timers by their timeout time. Whether we use an array or a list,

it will certainly also optimize our search for a timer. Put the timers whose times are near, that is, the earlier ones, at the front of the search. And then there's another one, which is using a linked list. Naturally, on the one hand, that also saves space.

And in this linked list, it is also quite flexible. Insertion and deletion are also quite flexible. especially when you want to do sorting, actually, if you use a linked list, the implementation is more flexible than with an array.

You can try implementing this on your own. So we have an exercise here, exercise 15-1 of this chapter, and what we really hope is that, building on what you've learned so far, you go and optimize, and make this piece of code a bit better.

This is the first part of our optimization. And actually, there's still room for optimization. For this timer, there is in fact still room for optimization. As we know, whether we implement it using an array, or using a linked list,

once there are many timers, the process of searching and locating, is simply unavoidable. So let's assume in this example you can see that this 135 here is this timers value Assume they are still sorted This side is still already sorted

Even if they are sorted, we know the search process still cannot be avoided And as this linked list or this array has more and more elements in it, the search time will keep growing Because everyone knows, in this situation, based on our data structures

algorithm complexity, if we analyze it, we've learned this before, right? The complexity of this algorithm, for something like a linked list and this kind of array, the complexity of its search should be what? Just a simple linear

array or linked list, its complexity is definitely O(N). The answer, right? So what does O(N) mean? It means that it's actually a X equals Y, Y equals X kind of effect. In other words, when the number of searches and the number of implementation steps of this algorithm

is directly proportional to the number of elements, the more elements you have, the more execution steps it's definitely growing linearly, a linear relationship. Now, this kind of linear growth, I should say, is not a very good

well, the efficiency is not very high, not very high at all. So how can we further optimize this software timer? Well, here's what we can actually do. On top of what we already have, we can basically add another layer on top of it.

Adding another layer, as you can see, if we add another layer, what kind of effect does that give us? It means it takes every fixed interval, this is essentially like the bottom, the very bottom is essentially level one, and then we abstract level two from it.

As for the second level, as for this second level, it basically means taking some of its element nodes inside, if we extract them out, it's somewhat like the effect of binary search, somewhat like the effect of binary search. Now, with this binary search kind of effect,

you can simply compare the two effects. Now, in the one below, as you know, it's still linear, meaning that to find this 20, we have to traverse from left to right one by one. And traversing one by one, inside here we have one, two, three, four, five, six, seven,

that is, it takes at least seven times, right? seven nodes. before we can find this node 20. Now, if we if, on this structure, we abstract this second layer, then our traversal process will actually be much faster. As you can see,

with this on top, 1, 5, 10, 1, 2, 3, 4 — four steps, and it can reach this. Right? Now, if we keep going with this idea, that is, if we find that there are more nodes at the bottom, and we build a third level, a third level, then the number of steps relative to this would be even smaller.

In other words, if it were three levels, that is, you first go from 1 to 11, at 11 we see that 20 is greater than 11, but smaller than 26. So we just retreat to its second-to-last level. Now down there is 17. 17 is bigger than it,

but if it's smaller than 26, then going one step further, we hit it very quickly. So this blue portion, the hit curve will actually be compared to the green one it will actually still be more it will be more So this kind of thing,

according to these numbers, this is actually like a pyramid structure going up. So if you look at it from top to bottom, you can actually see that it's really the concept of binary search, right? Doesn't it look a bit like binary search?

Now, as we know, with binary search, like the binary search method, Actually, it's also used quite often in data structures, right? Now, this kind of binary-search-like approach for linked lists, this kind of hierarchical level-by-level method,

we actually have a term for it. That is, this method, we call it the skip list algorithm. If you're interested, you can look up the basic idea of the skip list algorithm. Here, I'll just give you a brief overview. This is a way to do it.

For its search-hit algorithm, we can actually do some further optimization. That is, once we have this sorting, we can actually do more than just sort. As long as we have sorting as a basis, we can actually bring in binary search.

Now, with this kind of skip list approach, according to these estimates, We know that when we talk about binary search, it's usually the idea of log base 2, and so its curve, it's something like, you know, for log n, it looks like this,

and then log 2, log base 2, it's a curve like this, which means when n grows larger this way, the number of execution steps, its growth trend, is actually relatively much gentler than that one, which is a fairly good thing, when our element nodes,

like, say, our timers, the number of software timers, gets fairly large, then its number of execution steps, will be fewer. its growth trend becomes much smaller. This is the skip list idea. This is also one way for us to optimize software timers.

So I'll give you a brief introduction to it. The hope is really that you'll use software techniques to further optimize it. Now, our exercise 15-2 is actually implemented using this kind of skip list algorithm. You can go home and try implementing it.

That is to say, as you can see, when it comes to our operating system, beyond some of the basic concepts of the underlying hardware, a lot of what comes next actually involves the software side. And the more software there is,

the more data structures and algorithms can come into play. It's said that throughout the whole operating system, there's still quite a bit of algorithmic optimization to be done on the software side. Just something for you to be aware of.

Alright, so that's the overall summary of this exercise. Go back and try doing that together yourselves. So as for this software timer, we'll stop our introduction of it right here.
