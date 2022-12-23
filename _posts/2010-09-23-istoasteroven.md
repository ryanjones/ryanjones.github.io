---
title: isToasterOven?
categories: [Software Engineering]
tags: ['Smalltalk']
---


As I was cleaning up some old depreciated code in one of my works Smalltalk images I came across this method:
```smalltalk
isToasterOven
"---------------------------------------------
Description:
        Answer true if I am.
---------------------------------------------"
        ^(OS getSystemMetrics: SmSlowmachine) = 1
```

isToasterOven? isBestMethodEver?

Let me explain what this little ditty does. We have an object of OS which receives the message of getSystemMetrics: SmSlowmachine. SmSlowmachine is declared as 73. So basically the OS object goes out to the [Windows API][1] and calls GetSystemMetrics with a value of 73. This in turn then returns a value of 0(false)/1(true).

 [1]: http://en.wikipedia.org/wiki/Windows_API

Now you may ask, “why 73 Ryan?” and to which I’ll respond “I have no idea”. My very first google search came up with the page [GetSystemMetrics][2]. Success! I did a quick search on 73 and found this:

 [2]: http://msdn.microsoft.com/en-us/library/ms724385(VS.85).aspx

SM_SLOWMACHINE 73 – Nonzero if the computer has a low-end (slow) processor; otherwise, 0.

Hurrah! isToasterOven is actually checking if the machines IS a toaster oven! However, if you’re like me, you’re asking “what determines a nonzero value? How crappy does a machine have to be to return that?” and to which I’ll respond “Hell if I know! Off to the internet!”.

And to the internet I went. I searched high and low, but all I could find was [this][3] saying that it returns “True if machine is too slow to run Windows 95″. I’m not sure if this was one of those things that someone updated somewhere, and then everyone copied it all over the place, or if it’s the actual real deal. And if it’s the real deal, why wouldn’t Microsoft have it on their site?

 [3]: http://lbpe.wikispaces.com/GetSystemMetrics

If anyone can find out the exact specifics on this call please leave a comment on my blog! I’d love to find out what determines isToasterOven?.

In case you’re curious of the [Windows 95 minimum requirements][4]:

 [4]: http://support.microsoft.com/kb/138349

*   Personal computer with a 386DX or higher processor (486 recommended)
*   4 megabytes (MB) of memory (8 MB recommended)
*   Typical hard disk space required to upgrade to Windows 95: 35-40 MB 
*   Typical hard disk space required to install Windows 95 on a clean system: 50-55 MB 
*   One 3.5-inch high-density floppy disk drive
*   VGA or higher resolution (256-color SVGA recommended)

Back in the good ole days, they must have needed to know if the machine had the ability to run certain functions. Alas, this code wasn’t called from anywhere else in the system and was in line to be purged. This little piece of code had survived the years untouched. I just couldn’t bring myself to delete isToasterOven?.

May isToasterOven? live on forever.