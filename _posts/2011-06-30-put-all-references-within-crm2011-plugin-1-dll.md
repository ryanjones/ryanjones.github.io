---
title: Put all references within CRM2011 Plugin (1 DLL)
categories:
  - Microsoft / CRM / SharePoint / SSRS
---

We needed to come up with an alternate solution to ILMerge since we just couldn’t get it to work, or work consistently when creating plugins. We wanted to be able to take 1 .DLL and upload it to the server (via the plugin tool) to the database.

This method has only been test on CRM2011 plugins, but should work for many other scenarios. There are some pros and cons however.

Pros:

*   Everything is contained in 1 DLL.
*   Easier to deploy up to a server (no uploading dependencies separately) , or give to someone to use.

Cons:

*   The DLL can bloat to large sizes (with CRM you’ll have to [ increase the upload size so you can upload your plugin][1]).
*   If one of the DLL’s within your DLL get’s updated you have to recompile your whole DLL with the new DLL.
*   If you’re using Xrm.DLL in your plugin you’ll need to recompile your A LOT (because of the new fields being added to CRM)

 [1]: http://www.ryanonrails.com/2011/05/28/uploading-large-crm-2011-plugins/

We’ve used this method quite a bit now, and it works perfectly for us. We don’t use a lot of 3rd party vendors so we don’t have to recompile often.

Here we go:

I’m going to assume you’ve created a C# class library at this point. We need to edit our .csproj file.

Right click your project -> Click Unload Project (it will go grey and say unavailable).
Right click your project again -> Click .csproj

Scroll down to the bottom and find this line:
```xml
<Import Project="$(MSBuildToolsPath)\Microsoft.CSharp.targets" />
  <!-- To modify your build process, add your task inside one of the targets below and uncomment it.
       Other similar extension points exist, see Microsoft.Common.targets.
  <Target Name="BeforeBuild">
  </Target>
  <Target Name="AfterBuild">
  </Target>
  -->
```

Now paste this code underneath the comments (I like to keep the comments intact):
```xml
<Target Name="AfterResolveReferences">
  <ItemGroup>
    <EmbeddedResource Include="@(ReferenceCopyLocalPaths)" Condition="'%(ReferenceCopyLocalPaths.Extension)' == '.dll'">
      <LogicalName>%(ReferenceCopyLocalPaths.DestinationSubDirectory)%(ReferenceCopyLocalPaths.Filename)%(ReferenceCopyLocalPaths.Extension)</LogicalName>
    </EmbeddedResource>
  </ItemGroup>
</Target>
```

It should look something like this now:
```xml
<Import Project="$(MSBuildToolsPath)\Microsoft.CSharp.targets" />
<!-- To modify your build process, add your task inside one of the targets below and uncomment it.
     Other similar extension points exist, see Microsoft.Common.targets.
<Target Name="BeforeBuild">
</Target>
<Target Name="AfterBuild">
</Target>
-->
<Target Name="AfterResolveReferences">
  <ItemGroup>
    <EmbeddedResource Include="@(ReferenceCopyLocalPaths)" Condition="'%(ReferenceCopyLocalPaths.Extension)' == '.dll'">
      <LogicalName>%(ReferenceCopyLocalPaths.DestinationSubDirectory)%(ReferenceCopyLocalPaths.Filename)%(ReferenceCopyLocalPaths.Extension)</LogicalName>
    </EmbeddedResource>
  </ItemGroup>
</Target>
```

Now, you need to set **ONLY** the references that you want to copy into the DLL as “Copy Local” = true. As an example, I would like to include Xrm.DLL in my project, so it set it’s “Copy Local” to true.

![][3]

 [3]: /assets/img/old/Copy_Local_True.png

Now, we can reload our project by Right clicking our project -> Reload Project. Open up your plugin and add this event handler:

```csharp
private static Assembly OnResolveAssembly(object sender, ResolveEventArgs args)
{

  Assembly executingAssembly = Assembly.GetExecutingAssembly();
  AssemblyName assemblyName = new AssemblyName(args.Name);
  string path = assemblyName.Name + ".dll";

  if (assemblyName.CultureInfo.Equals(CultureInfo.InvariantCulture) == false)
  {
     path = String.Format(@"{0}\{1}", assemblyName.CultureInfo, path);
  }

  using (Stream stream = executingAssembly.GetManifestResourceStream(path))
  {
       if (stream == null)
           return null;
       byte[] assemblyRawBytes = new byte[stream.Length];
       stream.Read(assemblyRawBytes, 0, assemblyRawBytes.Length);
       return Assembly.Load(assemblyRawBytes);
  }
}
```
In a static constructor, we need to hook up the event handler. Add this code:
```csharp
static TestEntityPlugin()
{
  AppDomain.CurrentDomain.AssemblyResolve  = OnResolveAssembly;
}
```

You should now be able to compile your DLL! You won’t notice any differences in Visual Studio, but if you use .NET reflector to view the .DLL you’ll notice that the references (with “Copy Local” = true) are within the DLL now.

Basically what’s happening is when the DLL is used, it looks within itself to access for the reference instead of looking elsewhere.

Caveats:
The plugin must be registered as “Non-sandbox” in order to have the permissions to use the AssemblyResolve event handler.

![][4]

 [4]: /assets/img/old/Non_Sandbox.png

The user registering the plugin needs to be a deployment manager (otherwise users could inject bad stuff with AssemblyResolve, whether on purpose or by accident).

That should be it! A pretty slick solution. I don’t necessarily recommend putting Xrm.DLL into your plugin since it’s regenerated so often. Works great for dll’s like Microsoft.Xrm.**Client**.

Couple quick references:
This is where abunch of code came from (this was 1 exe with all of the dll’s within it):

Good information on AssemblyResolve:

A big shoutout to Donny for researching into this and the screen shots.
