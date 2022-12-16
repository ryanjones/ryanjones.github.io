---
title: 'Customize Sitemap Locales - CRM 2011'
categories: [Microsoft]
---


How to customize sitemap locales!

Depending on your locale you’d see the word Cookies in English, French or German (I used a translator for these :D)

```xml
<Area Id="The word Cookies in many languages" 
         ShowGroups="true" Icon="/_imgs/cookies.gif">
<Titles>
 <Title LCID="1033" Title="Cookies" />
 <Title LCID="1036" Title="Les Cookies" />
 <Title LCID="1031" Title="Gebäck" />
</Titles>
```


In theory you should be able to apply this concept to the main navigation.

Here’s a list of the locale’s:

http://msdn.microsoft.com/en-us/library/cc150884.aspx

There’s a blurb on Titles here (scroll down to “Titles and Title”):

http://rc.crm.dynamics.com/rc/regcont/en_us/op/articles/sitemap.aspx

In theory you should be able to apply this same concept to the Sub Area elements which are underneath Area -> Group -> Sub Areas (which are Sales Marketing Etc.)

Here’s the SubArea XML definition: 

http://msdn.microsoft.com/en-us/library/cc150884.aspx