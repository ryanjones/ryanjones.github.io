---
title: 'Update Custom Columns from SharePoint 2010 via SOAP Web Services  - UpdateListItems'
categories:
  - Microsoft / CRM / SharePoint / SSRS
---


Here’s a query to update a file’s custom columns in SharePoint 2010.
```csharp
// Build the CAML Query
System.Text.StringBuilder oSb = new System.Text.StringBuilder();
oSb.Append("     <Batch OnError=\"Continue\" >");
oSb.Append("         <Method ID=\"1\" Cmd=\"Update\">");
oSb.Append("             <Field Name=\"ID\">" + fileID + "</Field> ");
oSb.Append("             <Field Name=\"Custom_x0020_Column\">" + customColumnText + "</Field> ");
oSb.Append("             <Field Name=\"Test_x0020_123\">" + test123Text+ "</Field> ");
oSb.Append("        </Method>");
oSb.Append("    </Batch>");
 
string sResult = oSb.ToString();
XmlDocument CAMLqueryXML = new XmlDocument();
CAMLqueryXML.LoadXml(sResult);
 
// Execute UpdateListItems
System.Xml.XmlNode result = lists.UpdateListItems(libraryName, CAMLqueryXML);
```