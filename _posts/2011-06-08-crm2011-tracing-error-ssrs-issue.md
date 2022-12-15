---
title: 'CRM2011 Tracing Error - SSRS Issue'
categories:
  - Microsoft / CRM / SharePoint / SSRS
---


I was getting this System.Security.SecurityException because my SSRS datasource settings were incorrect:

```xml
Unhandled Exception: System.ServiceModel.FaultException`1[[Microsoft.Xrm.Sdk.OrganizationServiceFault,
Microsoft.Xrm.Sdk, Version=5.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35]]:
System.Security.SecurityException: Microsoft Dynamics CRM has experienced an error. Reference number for
administrators or support: #84ED43B9Detail:
<OrganizationServiceFault xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/xrm/2011/Contracts">
  <ErrorCode>-2147220970</ErrorCode>
  <ErrorDetails xmlns:d2p1="http://schemas.datacontract.org/2004/07/System.Collections.Generic" />
  <Message>System.Security.SecurityException: Microsoft Dynamics CRM has experienced an error. Reference number for administrators or support: #84ED43B9</Message>
  <Timestamp>2011-05-16T16:55:39.4637893Z</Timestamp>
  <InnerFault i:nil="true" />
  <TraceText>

[<PluginName>: Plugins.<PluginName>]
[d6afc2c3-717a-e011-9afc-00155d036888: Plugins.<PluginName>: Update of <Entity Name>]

</TraceText>
</OrganizationServiceFault>
```

Hope this helps!