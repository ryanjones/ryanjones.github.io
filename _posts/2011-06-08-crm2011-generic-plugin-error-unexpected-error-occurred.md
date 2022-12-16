---
title: 'CRM2011 Generic Plugin Error - "Unexpected error occurred."'
categories: [Microsoft]
---


I truly hope you don’t end up getting this error:

```xml
Unhandled Exception: System.ServiceModel.FaultException`1[[Microsoft.Xrm.Sdk.OrganizationServiceFault,
 Microsoft.Xrm.Sdk, Version=5.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35]]: An unexpected
 error occurred.Detail:
<OrganizationServiceFault xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/xrm/2011/Contracts">
  <ErrorCode>-2147220970</ErrorCode>
  <ErrorDetails xmlns:d2p1="http://schemas.datacontract.org/2004/07/System.Collections.Generic" />
  <Message>An unexpected error occurred.</Message>
  <Timestamp>2011-06-07T21:47:31.4167053Z</Timestamp>
  <InnerFault i:nil="true" />
  <TraceText i:nil="true" />
</OrganizationServiceFault>
```

I’m not sure why, but this exception seems to overwrite all of my tracing text. This error is basically a catch all of it can’t figure out what type of exception your plugin is throwing. Turns out that I was trying to generate a report in SSRS that was invalid (a relationship changed in CRM, but the report hadn’t been updated). The report caused a cataclysmic effect and the plugin crashes.

Event viewer wasn’t very helpful, and the tracing in the C:\Program Files\CRM Install\Trace wasn’t very helpful either. It was an easy fix for me (because it worked on at least 1 entity and I was able to narrow it down easily), but another option would be to comment out code until you start to get some tracing again.

Hope this helps!