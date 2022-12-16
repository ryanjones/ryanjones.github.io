---
title: Automatic migration was not applied because it would result in data loss.
categories: [Microsoft]
---


I’ve just started using EntityFramework.Migrations and came accross this error:

> No pending explicit migrations.  
> Applying automatic migration: 201205101614472_AutomaticMigration.  
>   
> Automatic migration was not applied because it would result in data loss. 
>  
 
It’s because my migrations would’ve nuked some of my data. I’m okay with that though, you just need to throw on the -Force tag to get it to complete the migration:

> Update-Database -Verbose -Force 
