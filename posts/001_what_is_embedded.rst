I Once Worked on an Embedded Project
====================================

What is Embedded Programming?
-----------------------------
Usually, when thinking about programming, people think about computers. A device with a keyboard, mouse and a screen.
For the past 10-or-so years we grown accustomed to our phones being much like computers. They are capable of running applications much like a regular laptop is capable,
and the latests phones may be more powerful than some - not very old - laptops. But actually, to run a program, all we need is love, and a processor.
If we have those two (and the first is crucial) we can run software. Any system that has a processor in it, but it is not a full keyboard-mouse-touchscreen computer, is an embedded system.

Where can we find it?
---------------------
Well, if you are not from the field, you will be surprised. Your home washing machine has some micro-controller in it, which is a type of processor. So your washer ia an embedded system.
You might think “how complicated can a washing machine be?” So I’ll tell you that I once had a repair guy for my washer,
and to fix the problem he plugged in a USB disk-on-key that upgraded the washer software and copied the log files for review with the manufacturer.
Of Course, it’s not just washing machines. Refrigerators, air conditioners, cars, cell phone antennas, all have embedded systems.

Washing machine Vs desktop - Round 1
------------------------------------
Because a CPU is a CPU, whether it is in a computer or a washing machine, programming it should be the same. But because a washing machine is not a computer, there are some differences.
First of all, a lot of embedded systems don’t have an operating system. Even the ones that do have an OS, it is not a fully featured OS like Windows.
The most you can expect is the Linux kernel with the very basic shell. Under these conditions, it would be very difficult to run an interpreter of any kind,
leading to the first limitation of embedded programming - you must use a fully compiled language. Well, I lied. Personally I worked on a system where we managed to run
python over embedded Linux and run python code in the embedded system. But that is the exception. 99.99% of the time, you need your code to be fully compiled to machine code.
Another difference is resources. A modern cell phone has 4GiB of RAM and a powerful quad-core CPU. Most laptops has more than that.
A washing machine however has most of its resources directed to its motor, water pump, and other components crucial for the washing operation itself.
When it comes to computing power, you should be happy if you get 0.5MiB of RAM, and a simple single core basic CPU equivalent in its power to a pentium processor from the early 2000s.
You need to write an efficient code, which compiles to efficient machine code. That rules out languages with features like garbage collector or memory bounds checks,
that result in a sub-optimal machine code.
The final difference I want to talk about is the hardware. In a regular computer, access to hardware such as the disk, or a network interface, is done with the help of the OS using it’s drivers.
That is possible because standrd computer hardware is very generic and in use by all computers. That way, the OS can have a hardware-accessing code that will work with most common hardware.
Now imagine your car breaking system. It has a very specific hardware. Specific for this particular car model. You will have to write the HW access code (the “driver”) yourself.
That requires your language to be very “low level”. You will have to have a syntax that allows you to access very specific memory addresses and execute very specific assembly instructions.

How to program a washing machine?
---------------------------------
So to summerise - an embedded system has very little resources, non-standard hardware and CPU and it can't use the very helpfull fetures of interperted languages like Python.
To deal with these limitations, programming an embedded system is doen using C language, sometime C++. The software and hardware engineers work closely together to extract
the maximum from the little resources the system has. Personally, I like this challenge. It is what this blog is all about.

Do it yourself?
---------------
From this post you may get the impression that inorder to do embedded programming, you have to work for a big company, with hardware development capbilities
and you can't experiment on your own. Well, that is not entierly true. Automotive manufaturers don't bother themselves with developing a CPU for their cars.
They buy them from someone with CPU development experties. Why shouldn't you be able to buy one your self? You may think it's expensive, manufaturing takes time,
or that is complicated to use. That was maybe true 20 yiers ago. Today you can by an arduino or rasbarie-pie boards. These are small development kits that
include a board with a CPU and many interfaces. You can program them yourself, or download many open-source projects that makes them do something.
You can use them for smart house dashboards, a cheap media streamer, or even to water your garden. You can learn from these projects and maybe create something of your own.

If you are femillier with embedded programming, I hope I conviced you that I have some interesting things to say about it. If not, I hope I convinced you it's interesting,
even if you won't go buying a brand new car breaking system to program yourself. Any case, hope to see you here for next posts.

