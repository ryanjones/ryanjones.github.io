---
title: Tracing error in CRM2011
categories: [Microsoft]
---


We recently moved to a new server and I was getting this error:

>Invalid Trace Directory. Additional Info:[ Unable to Write file , Trace directory not defined (Reporting
Process:File Name is Null. LocalTraceSettings: {Filename:  ,FileCountSuffix:1 ,TraceFileSize:10485760
,TraceDirectory: ,TracingCallStack:Yes ,IsTracingOff:No ,LoadState:LoadSuccessfulUnreported
,RefreshTraceInt:-1 ,SiteWideRefreshTraceInt:-1 ,RegistryRefreshTraceInt:-1} ] , AppDomain:CrmAsyncService)

Turns out this is caused from CRM Tracing not being setup (and my plugin was using tracing). Go here to set it up: [CRM Tracing Setup][1]

 [1]: http://support.microsoft.com/kb/907490

Hope this can help point you in the right direction.