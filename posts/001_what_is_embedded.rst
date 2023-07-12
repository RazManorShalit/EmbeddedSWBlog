I Once Worked on an Embedded Project
====================================

What is Embedded Programming?
-----------------------------
Usually, when thinking about programming, people think about computers - devices with a keyboard, mouse and a screen.
For the past 10-or-so years we have grown accustomed to our phones being much like computers.
They are capable of running applications much like a regular laptop is capable.
The latest phones may even be more powerful than some - not very old - laptops.
But actually, in order to run a program, all we need is love, and a processor.
If we have those two (and the first one is crucial) we can run software.
Any system that has a processor, but is not a fully featured keyboard-mouse-touchscreen computer, is an embedded system.

Where can we find it?
---------------------
Well, if you are not from the field, you will be surprised.
Your home washing machine has some micro-controller in it, which is a type of processor.
So your washer is an embedded system. You might think “how complicated can a washing machine be?”
So let me tell you about a time when I had called a repair technician for my washer.
In order to fix the problem he plugged in a USB disk-on-key that upgraded the washer software,
copied the log files and sent them to the manufacturer for review.  Of course, it's not just washing machines.
Refrigerators, air conditioners, cars, cell phone antennas, all have embedded systems.

Washing machine Vs desktop - Round 1
------------------------------------
Because a CPU is a CPU, whether it is in a computer or a washing machine, programming it should be the same.
But because a washing machine is not a computer, there are some differences.

Operating System
^^^^^^^^^^^^^^^^
First of all, a lot of embedded systems don't have an operating system.
Even for those that do have an OS, it is not a fully featured OS like Windows.
The most you can expect is the Linux kernel with a very basic shell. Usually you can't expect dynamic memory allocation,
virtual memory, file systems, and likewise, all of which are OS features.
Sometimes you can implement them yourself but that is extra work.

Compiled Programming Language
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Because there is no operating system, it would be very difficult to run an interpreter of any kind.
You must use a fully compiled language. Well, I lied. Personally I worked on a system where we managed to
run python over embedded Linux and run python code in the embedded system.
But that is the exception. 99.99% of the time, you need your code to be fully compiled to machine code.

Resources
^^^^^^^^^
A modern cell phone has 4GiB of RAM and a powerful quad-core CPU. Most laptops has more than that.
However, a washing machine has most of its resources directed to its motor, water pump,
and other components essential to the washing process itself.
When it comes to computing power, you should be happy if you get 0.5MiB of RAM and a simple single core basic CPU
equivalent in its power to a Pentium processor from the early 2000s.
You need to write an efficient code, which compiles to efficient machine code.
That rules out languages with features like garbage collector or memory bounds checks,
that result in a sub-optimal machine code.

Hardware
^^^^^^^^
The final difference I want to present is the hardware. In a regular computer,
accessing hardware components such as the disk, or a network interface,
is done with the help of the OS using it’s drivers. 
That is possible because standard computer hardware is very generic and in use by all computers.
That way, the OS can have an hardware-accessing code that will work with most common hardware.
Now imagine your car breaking system. It has a very specific hardware. Specific for this particular car model.
You will have to write the HW access code (the “driver”) yourself.
That requires your programming language to be very “low level”.
It will be necessary for the language to have a syntax that allows you to access very specific memory addresses
and execute very specific assembly instructions.

How to program a washing machine?
---------------------------------
So to summarize - an embedded system has very little resources, non-standard hardware and CPU.
It can't use the very helpful features of interpreted languages like Python.
To deal with these limitations, programming an embedded system is done using C language,
sometime C++, although there are exceptions. The software and hardware engineers work closely together to extract
the maximum from the little resources the system has. Personally, I like this challenge.
It is what this blog is all about.

Do it yourself?
---------------
From this post you may get the impression that in order to do embedded programming you have to work for a big company,
with hardware development capabilities and you can't experiment on your own. Well, that is not entirely true.
Automotive manufacturers don't bother themselves with developing a CPU for their cars.
They buy them from someone with a CPU development expertise. Why shouldn't you be able to buy one your self?
You may think it's expensive, manufacturing takes time, or that is complicated to use.
That was maybe true 20 years ago. Today you can buy an Arduino or Raspberry Pi boards.
These are small development kits that include a board with a CPU and many interfaces.
You can program them by yourself, or download many open-source projects that makes them do something.
You can use them for smart house dashboards, a cheap media streamer or even to water your garden.
You can learn from these projects and maybe create something of your own.

If you are familiar with embedded programming or not, I hope you find what I have to say about it interesting,
even if you won't go buying a brand new car breaking system to program yourself.
Any case, hope to see you here for next posts.
