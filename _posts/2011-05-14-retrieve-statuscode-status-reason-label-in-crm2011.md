---
title: Retrieve Statuscode (Status Reason) Label in CRM2011
categories: [Microsoft]
---

Here’s a quick snippet on how to get the Status Reason from within a CRM 2011 Plugin. The actual behind the scene name for Status reason is “statuscode”.

Terminology:
Status code value is a numeric value in a drop down list (ex: 859760603)
Status code label is a text value which corresponds to a numeric value (859760603 = “Active”)

```csharp
// References you will need
using Microsoft.Xrm.Sdk;
using Microsoft.Xrm.Sdk.Client;
using Microsoft.Xrm.Sdk.Messages;
using Microsoft.Xrm.Sdk.Metadata;

// I've omitted the namespace and class name due to lots
//  of indentation
public void Execute(IServiceProvider serviceProvider)
 {

IPluginExecutionContext context;
IOrganizationService service;
OrganizationServiceContext serviceContext;

// Setup Service Factory
IOrganizationServiceFactory serviceFactory =
    serviceProvider.GetService(typeof(IOrganizationServiceFactory)) as
    IOrganizationServiceFactory;

// Set context
context = serviceProvider.GetService(typeof(IPluginExecutionContext)) as
    IPluginExecutionContext;

// Set Service
service = serviceFactory.CreateOrganizationService(context.UserId);

// Set Service Context
serviceContext = new OrganizationServiceContext(service);

// Create the entity from Target
Entity entity = (Entity)context.InputParameters["Target"];

// Get the numeric value of our current status code
OptionSetValue entityStatusCode =
    (OptionSetValue)entity.Attributes["statuscode"];

// Create the attribute request (define which entities attribute
//  information we want to get) entity.LogicalName is our current
//  name of our entity (ex: contact, account, etc.)
RetrieveAttributeRequest attributeRequest = new RetrieveAttributeRequest
{
    EntityLogicalName = entity.LogicalName,
    LogicalName = "statuscode",
    RetrieveAsIfPublished = true
};

// Execute the request to get the attribute information
RetrieveAttributeResponse attributeResponse =
    (RetrieveAttributeResponse)serviceContext.Execute(attributeRequest);
AttributeMetadata attrMetadata =
        (AttributeMetadata)attributeResponse.AttributeMetadata;

// Cast the AttributeMetadata to StatusAttribute data
StatusAttributeMetadata statusAttrMetadata =
    (StatusAttributeMetadata)attrMetadata;

// Get the status code label by comparing all of the status code values
//  possible for the status code drop down list. Once we get a match on
//  the value we take the label for that value and assign it to our
//  string variable
string statusCodeLabel = "";

// For every status code value within all of our status codes values
//  (all of the values in the drop down list)
foreach (StatusOptionMetadata statusMeta in
    statusAttrMetadata.OptionSet.Options)
{
    // Check to see if our current value matches
    if (statusMeta.Value == entityStatusCode.Value)
    {
        // If our numeric value matches, set the string to our status code
        //  label
        statusCodeLabel = statusMeta.Label.UserLocalizedLabel.Label;
    }
}

} // End of Execute method
```

That should be it.