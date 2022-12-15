---
title: CRM2011 Plugin Timeout Troubleshooting
categories:
  - Microsoft / CRM / SharePoint / SSRS
---


My plugin was set to fire when an update of a drop down happened (it then checks the value and deals with it accordingly). My plugin called an SSRS webservice to generate a report on that same entity. Whenever I would save I’d get this error:

```xml

Unhandled Exception: System.ServiceModel.FaultException`1[[Microsoft.Xrm.Sdk.OrganizationServiceFault, Microsoft.Xrm.Sdk, Version=5.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35]]: The plug-in
execution failed because the operation has timed-out at the Sandbox Client.
System.TimeoutException: Microsoft Dynamics CRM has experienced an error. Reference number for administrators or support: #346A60DEDetail:
<OrganizationServiceFault xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/xrm/2011/Contracts">
  <ErrorCode>-2147204751</ErrorCode>
  <ErrorDetails xmlns:d2p1="http://schemas.datacontract.org/2004/07/System.Collections.Generic" />
  <Message>The plug-in execution failed because the operation has timed-out at the Sandbox Client.
System.TimeoutException: Microsoft Dynamics CRM has experienced an error. Reference number for administrators or support: #346A60DE</Message>
  <Timestamp>2011-05-20T17:35:21.4703676Z</Timestamp>
  <InnerFault i:nil="trTue" />
  <TraceText>

[<Plugin Name>: Plugins.<Plugin Name>]
[0979224d-9f80-e011-a4cd-00155d03641b: Plugins.<Plugin Name>: Update of <Entity Name>]
</TraceText>
</OrganizationServiceFault>
```

The error threw me down a rabbit hole. I had no idea whether it was an SSRS timeout (taking so long to generate the report) or if it was something related to using the sandbox area for my plugin. I spent time making sure that SSRS was set to timeout > 2 minutes (there’s 2-3 places to set this). Then I also made sure the IIS settings for the CRM server (web.config) had a > 2 minute timeout.

None of that solved my problem. I then focused on the sandbox client. When I looked in the event viewer I kept getting a warning from MSCRMPlatform that looked like this:

**Query execution time of 119.1 seconds exceeded the threshold of 10 seconds. Thread: 30; Database: Database_MSCRM; Query: select
top 15001 “entity”.column as “column”
Query execution time of 120.8 seconds exceeded the threshold of 10 seconds. Thread: 30; Database: Database_MSCRM; Query: select
top 15001 “entity”.column as “column”

I thought it might be timing out. I couldn’t find much on the sandbox client limitations so I tried registering it outside of the sandbox and it still failed. All of the other plugins were working just fine, so I ruled out CRM and the sandbox service. The SSRS web service worked fine from a stand alone app, but broke in the plugin.

I felt that I had narrowed it down to the database at this point, so I did some investigation on locking. Using this query I was able to narrow down what was exactly causing the issue:
```sql
USE Database_MSCRM
DECLARE @locks TABLE (spid SMALLINT, dbid SMALLINT, objid INT, indid SMALLINT, TYPE NCHAR(4), resource NCHAR(32),
                      mode nvarchar(8), STATUS nvarchar(5))

INSERT INTO @Locks (spid, dbid, objid, indid, TYPE, resource, mode, STATUS)
EXEC sp_lock

SELECT * FROM @Locks
WHERE objid = ###########
```

Here’s output of of my sp lock. Click for a bigger image. Run object\_id) to get the name of the table that’s causing the lock (from the Locks table below). Ex: select object\_name(1018590817)

![][2]

The read of the table (SSRS, the IS lock) conflicts with the IX lock (the drop down update).

The update has IX (intent exclusive) and is granted a page lock on 1:2288 and the reason (SSRS) has IS (intent shared) and is also granted a page lock.

An intent shared lock prevents an exclusive lock and an intent exclusive lock prevents an intent shared. I’m not sure why this happens, since my plugin is post execution, but it probably has to do with the small amount of data, and the page locks grabbing at the same time.

Luckily there was a simple fix to the problem. In the report query dataset (written in FetchXML) make sure to have the no-lock=”true” option set, like so:




This makes it so your plugin won’t put any locks on the database. This took quite awhile to figure it out. As with any database locking problems the results were skewed all the time. I could hit save 3 times in a row and it would start working, or I would get a different error message every time.

Big thanks to Mathieu for keeping me sane and looking up the no-lock option in fetchxml.

I didn’t find many of these error messages on Google, so hopefully this has provided some insight into what could be happening in your system.


 [2]: /assets/img/old/CRM2011_Lock.png