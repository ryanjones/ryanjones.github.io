---
title: 'Build gems that require native extensions on Windows - Install gems in Windows with builds tools via MinGW'
categories: [Development]
tags: ['Ruby on Rails']
---


Ever get this error when either trying to install a gem, or running a bundle update (someone’s updated the gemfile)? 

>Fetching: linecache19-0.5.12.gem (100%)  
ERROR: Error installing linecache19:  
The ‘linecache19′ native gem requires installed build tools.

Please update your PATH to include build tools or download the DevKit from http://rubyinstaller.org/downloads and follow the instructions at http://github.com/oneclick/rubyinstaller/wiki/Development-Kit

Going to those links and setting everything up is way to much effort. Here’s a quick way to get everything working. We’re going to use MinGW (Minimalist GNU for Windows) to compile our gems.

Head over to [http://www.mingw.org/wiki/Getting_Started][1] and read under “Graphical User Interface Installer” (since we love our GUI’s :D). Don’t let the age of that post (2007) scare you off. MinGW is kept up to date. Here’s a direct [link][2] for download. Get the latest version. As of writing I’m using mingw-get-inst-20110530.exe.

 [1]: http://www.mingw.org/wiki/Getting_Started "http://www.mingw.org/wiki/Getting_Started"
 [2]: http://sourceforge.net/projects/mingw/files/Automated MinGW Installer/mingw-get-inst/ "link"

Run the .exe, click Next, Next, Make sure “Use pre-packaged repository catalogs” is selected, Select the agreement, I use the default C:\MinGW on the next step, Next on the start menu, I use the following: 

*   C Compiler
*   ObjC Compiler
*   MSYS Basic System
*   MinGW Developer ToolKit

Click Next and a dos window will popup doing it’s magic. Click Finish once it’s done.

Now click Start -> All Programs -> MinGW -> MinGW Shell

Once the shell is open punch in “gem install linecache19″ and you get this beautiful output:

>$ gem install linecache19  
Building native extensions. This could take a while…  
Successfully installed linecache19-0.5.12  
1 gem installed  
Installing ri documentation for linecache19-0.5.12…  
Installing RDoc documentation for linecache19-0.5.12…

Now you can install gems that have native extensions! And you have a new linux style environment to play around with!

You now have a new linux shell to add to your arsenal. There’s also a way to add MinGW to your path so you don’t have to run the MinGW shell, but I personally like to keep my path clean.

This should work for Windows XP, Windows Vista and Windows 7. 32 & 64 bit.