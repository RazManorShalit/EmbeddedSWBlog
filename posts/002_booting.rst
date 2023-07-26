I Once Worked on a Booting System
=================================
While planing this blog I found that many of the things I want to write about
relate to software in general, not just embedded. That is not surprising, after
all - "Embedded Software **is a** Software". But still, this is an embedded software
blog, so I want to start with an embedded specific topic - Booting.

What is Booting
^^^^^^^^^^^^^^^
When I first started learning programming, I was taught that all programs run
from the RAM. The OS loads the code from the disk to the RAM and instructs the
CPU to jump to the program location and start running. That was an OK explanation
for my 14 years old self, but very soon after, I noticed something - the OS is
a program too. Who loads that to the RAM from the disk? Someone, I don't remember
who, told me it is the BIOS but now I'm smarter - if the BIOS is also a program
it has the same problem as the OS, and no matter what program solves this problem,
that program has to be loaded. There is no end to this recursion.

This type of initializing problems, that we can't solve without something that
introduces the same problem, are called **Bootstrap** problems. In software,
The process that solves initializing bootstrap problem is called **booting**.
The software module that does the booting is called **Boot Loader**

Here is another example from the mechanical world - a classic car engine pistol
pulls the fuel into the engine as it moves down and compresses it as it moves up.
The compression causes the fuel to ignite, which pushes the pistol back down to restart the cycle.
The up and down moves are what making the wheels spin. When we first start the
engine, what makes the pistol make it's first move down?

Old cars actually had a manual handle you had to spin by hand in order to start
the process. Modern cars has a starter - an electronic device capable for emitting
high electrical current that starts the engine.

Where are We Running From?
^^^^^^^^^^^^^^^^^^^^^^^^^^
The car example is obviously over-simplifying how engines work, please don't crucify
me over that. But it does demonstrate how a bootstrap problem is solved by other
means to start the chain. In software we need something else to start the boot
process without the need to "load to RAM and jump". That something is an
Execute-in-Place storage.

Execute-in-Place (XiP) is a persistent storage (like an hard drive) which has a "memory" address.
it is not RAM, but the processor can access it as if it was regular RAM.
Unlike regular RAM, it maintains it's content when shut down and is, in most cases,
read only memory. That is not to say it can't ever be changed, but it is a complicated
process, and can't happen by simple write instruction to an address. That is
usually OK since changing the code as it is running is extremely bad practice
(but I did stuff like that as well so...).

There are many types of persistent storage devices that can be used as XiP devices
for boot code. The most simple one in my opinion is the ROM. That is a memory
that is integrated on the processor chip itself. It is not an extra device on the
board. Because it is part of the silicon, it's content is set during the manufacturing
of the chip. That makes it very cheep on both price and space on the chip.
The problem is that it requires the boot code to exist and fully debugged and
tested before manufacturing. Most software development is done after the chip
and board exist so it poses hard limitations on the developers.

There are types of ROM that can overcome the fact that it's content is pre
determined in manufacturing time. The PROM and the EEPROM. These are ROMS that
are blank after the chip manufacturing. Then the company that integrate the chips
to a product is able to program them (The P in PROM and EEPROM stands for Programable).
Historically the PROM is one shot, after programming its impossible to change it,
and the EEPROM (Electronically Erasable PROM) is able to "go blank" again with
under special electronic conditions which the manufacturer should know how to create.
Today the terms are used interchangeably, just to get confusing.

For example, I once worked with an ARM processor on a pre-made chip we bought
from a third party company. This chip had both ROM and EEPROM. The ROM was
only able to read from an SD card interface and copy it's content to the EEPROM.
The SD card should have contained a proper boot code. To make the processor boot
from the EEPROM instead of the ROM one of the electrical lines to the processor
had to be pulled down to 0. We put a jumper on the board for that.

.. digraph:: BootProcess

    rankdir="LR";
    splines=polyline;

    Processor [shape=square];

    subgraph {
        rank=same;
        nodesep=10;

        ROM [shape=square];
        EEPROM [shape=square];
    }
    SDCard;

    Processor -> ROM;
    Processor -> EEPROM;

    ROM -> SDCard;
    SDCard -> ROM;
    ROM -> EEPROM [style="dashed"];

Here we see a boot process using two devices, that the first (ROM) is used in
development time, and the second (EEPROM) is used in production time. That is
more complicated than the simple process we started with, to show how complicated
these problems could get.

Another type of XiP device is a FLASH memory. Flash memories are very common and
you may know them from your SSD drive or SD card in your phone. These are all
based of flash memories. FLASH memories are more expensive than ROMs and have to
be on a separate chip, but they are able to contain much more data. It is much
simpler to (re)program them than ROMS, but the number of reprograms is limited.
Done too much and the flash gets worn out and unusable. Not all flash devices
are XiP enabled - it is something to check if you plan your own board.

Money (Not) for Nothing
^^^^^^^^^^^^^^^^^^^^^^^
Cost is an issue, so we want to make our system as cheep as possible. But the
least expensive option - ROM - has the downside of being space limited and
complicated (maybe impossible) for change. Because we have limited space we will
make our boot loader small - meaning it has limited features. Because we can
never change it we have to make sure it is bugless - meaning it has very little
features and use cases we have to test.

A small and bugless boot loader is the goal. You might think it's no big deal.
The boot loader only needs to copy the OS (or application) from somewhere to the
RAM and jump there, right? what extra features are there that we have to give up
in the boot loader? What bugs could there possibly be in such code?

Well, here is one example of a bug in the boot loader. I once worked on a
project where we had two RAM areas - the main memory was very close to the
processor (the processor had direct access to it) so accessing it was fast.
The auxiliary memory was very far from the processor (it had to go through some
routing on the chip to access it) so accessing it was very slow. We could select
where the boot loader will load our code to but never tested the auxiliary case
because "it is slow and why would we want to run from there?". Well, come the
time where we noticed we need to load to the auxiliary memory and when we did
that, it didn't work. The problem was that the boot loader didn't initialize the
auxiliary memory, it remained in reset so we couldn't write to it. It was too late
to fix the problem so we had to find some workaround. Not that fun.

What to do About it?
^^^^^^^^^^^^^^^^^^^^
So what features we want from a boot loader? First thing - it has to read the
OS code from somewhere - where from? The obvious answer is some kind of storage,
like an SD card or other flash device. We could also want to get the OS code from
the cloud - that could be nice for easy upgrade or testing (no flashing for every change yahyee).
We can also go the old fashion way and receive the OS code from a serial line.
The format of the OS can also vary - is it compressed? encrypted? Is it bulk or
support dynamic loading? Possibilities are endless. And then we have to think about
software upgrades. Do we keep a fallback version? A previous version? How does
the boot loader know what version to boot. These are all very useful yet expensive
features we can't afford to put on our ROM.

To keep the boot loader small and bugless, but still fully featured, we break it apart.
This shouldn't be surprising, this what we do for all big problems. The boot
will take place in stages. Every stage is called a level. So the first stage is called
"Boot Loader Level 1", the second one "Boot Loader Level 2" and so on.
The first stage is sometimes called "Boot ROM" if it is in the ROM.

In the Level 1 we will do the bare minimum to allow loading the second stage.
The second stage will reside in a storage that is larger and more easialy writeable
than the ROM. A flash storage is a common choice for that stage. The second stage
will have all the features we want from our boot loader, such as multiple boot
sources, encryption, compression, SW upgrade and so on. Common second stage boot loaders
you may have heard of are GRUB for linux based PCs and servers, and UBoot for
embedded Linux systems.  

Lets return to our example. The system we were working on was an Embedded Linux
system with a UBoot second stage boot loader. The UBoot had 3 versions, a default
version that was read only, a current version and a previous version. The boot ROM
initialized all the RAM and the flash access where the UBoot was. Then it read
a special byte from the flash indicating what version of the UBoot to load.
Then it loaded UBoot to the RAM and ran it. The UBoot had a configuration file 
specifying where to read the OS from. It had file system capabilities so we could
save any number of OS versions we wished, as long as we had enough space on the flash.
We could also boot of the network or serial port for debugging. Putting it all
together gave something like this:

.. digraph:: BootProcessFull

    
    rankdir="LR";
    splines=polyline;

    Processor [shape=square];
    
    subgraph {
        rank = same
        
        ROM [shape=square];
        EEPROM [shape=square];
    }
    
    SDCard;

       FLASH [shape=none margin=0 label=<
        <TABLE border="0" cellspacing="0">
            <TR><TD PORT="default_uboot" border="1">Default UBoot</TD></TR> 
            <TR><TD PORT="uboot_ver1" border="1">UBoot Ver 1</TD></TR> 
            <TR><TD PORT="uboot_ver2" border="1">UBoot Ver 2</TD></TR> 
            <TR><TD PORT="linux" border="1">Embedded Linux</TD></TR> 
        </TABLE>
    >];

    POINT1 [shape="point"];

    Processor -> ROM;
    Processor -> EEPROM;

    ROM -> SDCard;
    SDCard -> ROM;
    ROM -> EEPROM [style="dashed"];
    EEPROM -> POINT1 [arrowhead="none"];
    
    POINT1 -> FLASH:default_uboot [arrowhead="dotnormal" label="Switch By a Byte\non the Flash" fontsize=8];
    POINT1 -> FLASH:uboot_ver1 [arrowhead="odot" style="dashed"];
    POINT1 -> FLASH:uboot_ver2 [arrowhead="odot" style="dashed"];

    FLASH:default_uboot:e -> FLASH:linux:e;

The End?
^^^^^^^^
I started this blog with a post the "beginning of the world" of an embedded system.
That is not to say that the first thing you do in development of an embedded system
is the bootloader(s). There are many open source and third-party solutions for this
problem, most of which are customable enough to adjust to your particular system.
You can skip the booting and start developing your application by using debuggers
and emulators - but that is for another time. Most developers will not work on
these problems, but it is important to know how this process works because it
affects the system behavior and capabilities, so you might need to mess around
with your bootloader, even if just to understand something.

Another importand asspect of this is that booting is a problem common to all
computer systems, embedded or not. That is to say - I may wanted to start with
something embedded specific, but I still found that embedded software **IS**, after all, **A** software.

Next one is about a nice bug I had in a bootloader I worked on, hope to see you there.