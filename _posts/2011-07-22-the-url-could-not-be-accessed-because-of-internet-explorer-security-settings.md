---
title: The URL could not be accessed because of Internet Explorer security settings
categories: [Microsoft]
---


I just ran into this when trying to validate a SharePoint (2010) site from within CRM2011. Here’ what I did to solve it:

*   Add the site you’re trying to validate against (https://sp.contoso.com) to your trusted sites in IE.
*   Drop the trusted sites level to **Low**
*   Click validate (which then the site should validate)
*   Make sure to set your trusted sites back up to **Medium **when done.

Hope this helps!
