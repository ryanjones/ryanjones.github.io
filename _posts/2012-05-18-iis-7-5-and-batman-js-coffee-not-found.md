---
title: 'IIS 7.5 and batman.js - .coffee not found'
categories: [Development]
tags: ['CoffeeScript']
---


If you’re trying to get batman.js trying to run within visual studio & IIS 7.5 you’re going to run into some issues serving up coffeescript files to batman.

> The error might look something like this:
>
> Failed to load resource: the server responded with a status of 404 (Not Found)
>

From what I understand, IIS doesn’t know what type of MIME type coffeescript is, so it doesn’t serve it out, here’s what you need in your web.config for it to work properly (put in my whole web.config):

```xml
<configuration>
  <system.web>
    <compilation debug="true" targetFramework="4.0"/>
  </system.web>

  <system.webServer>
    <httpErrors errorMode="Detailed" />
    <staticContent>
      <mimeMap fileExtension=".coffee" mimeType="coffeescript" />
    </staticContent>
  </system.webServer>
</configuration>
```
I gleaned some information from [this SO article][1].

 [1]: http://stackoverflow.com/questions/9760034/what-causes-a-404-4-on-iis-7-5-for-delivering-a-static-file "this SO article"
