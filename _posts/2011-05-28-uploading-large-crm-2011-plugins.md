---
title: 'Uploading Large CRM 2011 Plugins - Increase CRM 2011 Plugin Upload Size'
categories:
  - Microsoft / CRM / SharePoint / SSRS
---


You may notice that if you try and upload(register) a plugin > 1024 KB you’ll get an error similar to this (sorry for the shoddy formatting):

```csharp

Unhandled Exception: System.ServiceModel.CommunicationException: An error occurred while receiving the HTTP response to http://organization/RBR/XRMServices/2011/Organization.svc. This could be due to the service endpoint binding not using the HTTP protocol. This could also be due to an HTTP request context being aborted by the server (possibly due to the service shutting down). See server logs for more details.

Server stack trace:
   at System.ServiceModel.Channels.HttpChannelUtilities.ProcessGetResponseWebException(WebException webException, HttpWebRequest request, HttpAbortReason abortReason)
   at System.ServiceModel.Channels.HttpChannelFactory.HttpRequestChannel.HttpChannelRequest.WaitForReply(TimeSpan timeout)
   at System.ServiceModel.Channels.RequestChannel.Request(Message message, TimeSpan timeout)
   at System.ServiceModel.Channels.SecurityChannelFactory`1.SecurityRequestChannel.Request(Message message, TimeSpan timeout)
   at System.ServiceModel.Channels.ServiceChannel.Call(String action, Boolean oneway, ProxyOperationRuntime operation, Object[] ins, Object[] outs, TimeSpan timeout)
   at System.ServiceModel.Channels.ServiceChannelProxy.InvokeService(IMethodCallMessage methodCall, ProxyOperationRuntime operation)
   at System.ServiceModel.Channels.ServiceChannelProxy.Invoke(IMessage message)

Exception rethrown at [0]:
   at System.Runtime.Remoting.Proxies.RealProxy.HandleReturnMessage(IMessage reqMsg, IMessage retMsg)
   at System.Runtime.Remoting.Proxies.RealProxy.PrivateInvoke(MessageData msgData, Int32 type)
   at Microsoft.Xrm.Sdk.IOrganizationService.Update(Entity entity)
   at Microsoft.Xrm.Sdk.Client.OrganizationServiceProxy.UpdateCore(Entity entity)
   at PluginRegistrationTool.RegistrationHelper.UpdateAssembly(CrmOrganization org, String pathToAssembly, CrmPluginAssembly assembly, PluginType[] type) in C:\CRM2011sdk\sdk\tools\pluginregistration\RegistrationHelper.cs:line 263
   at PluginRegistrationTool.PluginRegistrationForm.btnRegister_Click(Object sender, EventArgs e) in C:\CRM2011sdk\sdk\tools\pluginregistration\PluginRegistrationForm.cs:line 391
Inner Exception: System.Net.WebException: The underlying connection was closed: An unexpected error occurred on a receive.
   at System.Net.HttpWebRequest.GetResponse()
   at System.ServiceModel.Channels.HttpChannelFactory.HttpRequestChannel.HttpChannelRequest.WaitForReply(TimeSpan timeout)
Inner Exception: System.IO.IOException: Unable to read data from the transport connection: An existing connection was forcibly closed by the remote host.
   at System.Net.Sockets.NetworkStream.Read(Byte[] buffer, Int32 offset, Int32 size)
   at System.Net.PooledStream.Read(Byte[] buffer, Int32 offset, Int32 size)
   at System.Net.Connection.SyncRead(HttpWebRequest request, Boolean userRetrievedStream, Boolean probeRead)
Inner Exception: System.Net.Sockets.SocketException: An existing connection was forcibly closed by the remote host
   at System.Net.Sockets.NetworkStream.Read(Byte[] buffer, Int32 offset, Int32 size)
```

One of the potential reasons for getting this error could be that your plugin is over 1024 KB.

This is caused by a few lines in the web.config located at C:\Program Files\Microsoft Dynamics CRM\CRMWebweb.config

Inside that file scroll down and look for something like this:

```xml
<!--Specific settings for the MSCRMServices directory-->
  <location path="MSCRMServices">
    <system.web>
      <httpRuntime maxRequestLength="8192" />
```

Since the Plugin Registration tool uses web services, this is the area we want to edit. You’re going to want to change this line here:

```xml
<httpRuntime maxRequestLength="8192" />
```


To:

```xml
<httpRuntime maxRequestLength="8192" requestLengthDiskThreshold="81920"/>
```

The trick here is that maxRequestLength is measured in **kilobytes** where as requestLengthDiskThreshold is measured in **bytes**. The above will give you up to 8 megs for your plugin (increase accordingly if your plugin is larger).

Why did you get the error though? Well, what requestLengthDiskThreshold does is determines how much of the request to hold in memory. If it goes over the requestLengthDiskThreshold it starts to write it temporarily to disk. The user account (or service) that’s being used doesn’t have the permission to write to wherever it’s buffering to and it crashes out. The default for requestLengthDiskThreshold is 1024KB (which is when it would start buffering it to disk).

I would’ve loved to spend the time to find out where and by who it’s being buffered to but unfortunately as of right now I just don’t have the time. If you know what permissions need to be set where please leave a comment. This might not be the most elegant solution, but it solves the issue for now. Be prepared to wait quite a bit longer while the file uploads via the Plugin Registration tool.

This is valuable knowledge for my upcoming post on how to hook many dll’s into 1 dll, which can cause the plugin to bloat into the 6-8 meg range.

**Edit July 20,2011** – This also fixes the error “The page was not displayed because the request entity is too large” in IIS7 when trying to upload large CRM 2011 reports.


