---
title: 'Import CRM 2011 Organization Error - You must map your Active Directory user account...'
categories: [Microsoft]
---


I ran across this error when I was moving an organization from 1 domain into another:

>You must map your Active Directory user account to at least one enabled Microsoft Dynamics CRM User who has the System Administrator security role before the organization can be imported.

![][2]

 [2]: /assets/img/old/import-fail.bmp

Basically what it is saying is that at least one of the accounts that exists in the database that you’re importing needs to be mapped to a Deployment Administrator in your current domain.

To set up a “true” Deployment Administrator:

For some reason I wasn’t able to install unless I was logged in as the Deployment Administrator that I was mapping (I tried logging in using a Deployment Administrator that didn’t exist in the CRM DB and it didn’t work for some odd reason). Could have been coincidence, but figured I’d document it just in case.

As far as mapping the users during the import:
Select custom mapping options-> Manually map users -> Match an account with a Deployment Administrator

It’s important to note that the user mapping doesn’t have to match the same name. For example:

![][3]

 [3]: /assets/img/old/user-mapping.png

So the user Ryan Jones can map to the CRM Deployment Admin. I don’t necessarily suggest this though. I would make one of the users a Deployment Admin and map the users directly. In the end I made Ryan Jones a Deployment Admin and mapped Ryan Jones (CRM DB) -> Ryan Jones (new domain).

Hope this helps!