---
title: Error installing redcar? Install Java!
categories: [Software Engineering]
tags: ['Ruby on Rails']
---


Are you receiving this redcar error?

>Successfully installed redcar-0.11  
1 gem installed  
Installing ri documentation for redcar-0.11…  
Installing RDoc documentation for redcar-0.11…  

>C:\Users\OwnerDesktop\Console2>redcar install 
Redcar 0.11 ( i386-mingw32 )  
found latest XULRunner release version: 1.9.2.16  
Downloading >10MB of binary assets. This may take a while the first time.  
Precaching textmate bundles…  
java -Xbootclasspath/a:”C:/Users/Owner/.redcar/assets/jruby-complete-1.5.3.jar” -Dfile.encoding=UTF8 -Xmx320m -Xss1024k -Djruby.memory.max=320m -Djruby.stack.max=1024k org.jruby.Main “C:/Ruby192/lib/ruby/gems/1.9.1/gems/redcar-0.11/bin/redcar” –no-sub-jruby –ignore-stdin –no-gui –compute-textmate-cache-and-quit –multiple-instance –start-time=1302709191  
C:/Ruby192/lib/ruby/gems/1.9.1/gems/redcar-0.11/lib/redcar/installer.rb:140:in “’: No such file or directory – java -Xbootclasspath/a:”C:/Users/Owner/.redcar/assets/jruby-complete-1.5.3.jar” -Dfile.encoding=UTF8 -Xmx320m -Xss1024k -Djruby.memory.max=320m -Djruby.stack.max=1024k org.jruby.Main “C:/Ruby192/lib/ruby/gems/1.9.1/gems/redcar-0.11/bin/redcar” –no-sub-jruby –ignore-stdin –no-gui –compute-textmate-cache-and-quit –multiple-instance –start-time=1302709191 (Errno::ENOENT)  
from C:/Ruby192/lib/ruby/gems/1.9.1/gems/redcar-0.11/lib/redcar/installer.rb:140:in `block in precache\_textmate\_bundles’  
from C:/Ruby192/lib/ruby/gems/1.9.1/gems/redcar-0.11/lib/redcar/runner.rb:80:in `construct_command’  
from C:/Ruby192/lib/ruby/gems/1.9.1/gems/redcar-0.11/lib/redcar/installer.rb:139:in `precache\_textmate\_bundles’  
from C:/Ruby192/lib/ruby/gems/1.9.1/gems/redcar-0.11/lib/redcar/installer.rb:26:in `install’  
from C:/Ruby192/lib/ruby/gems/1.9.1/gems/redcar-0.11/bin/redcar:25:in `‘  
from C:/Ruby192/bin/redcar:19:in `load’

**Install java**

I got this error after setting up a brand new virtual machine which didn’t have java on it. Once I installed java I was good to go :)

>C:\Users\OwnerDesktop\Console2>redcar install  
Redcar 0.11 ( i386-mingw32 )  
found latest XULRunner release version: 1.9.2.16  
Downloading >10MB of binary assets. This may take a while the first time.  
Precaching textmate bundles…  
java -client -Xbootclasspath/a:”C:/Users/Owner/.redcar/assets/jruby-complete-1.5.3.jar” -Dfile.encoding=UTF8 -Xmx320m -Xss1024k -Djruby.memory.max=320m -Djruby.stack.max=1024k org.jruby.Main “C:/Ruby192/lib/ruby/gems/1.9.1/gems/redcar-0.11/bin/redcar” –no-sub-jruby –ignore-stdin –no-gui –compute-textmate-cache-and-quit –multiple-instance –start-time=1302747689

Install instructions here:
https://github.com/redcar/redcar/wiki/installation
