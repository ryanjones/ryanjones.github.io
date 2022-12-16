---
title: 'MVC3 - Make sure that the controller has a parameterless public constructor.'
categories: [Microsoft]
---

I recently ran into this issue when I was accessing my WEBapi controller. I’m actually using MVC4, but I have ran into the same error in both MVC3 and MVC4.

```csharp
System.InvalidOperationException

An error occurred when trying to create a controller of type 'EST.Controllers.API.TemplatesController'. Make sure that the controller has a parameterless public constructor.


at System.Web.Http.Dispatcher.DefaultHttpControllerActivator.Create(HttpControllerContext controllerContext, Type controllerType)
 at System.Web.Http.Dispatcher.DefaultHttpControllerFactory.CreateInstance(HttpControllerContext controllerContext, HttpControllerDescriptor controllerDescriptor)
 at System.Web.Http.Dispatcher.DefaultHttpControllerFactory.CreateController(HttpControllerContext controllerContext, String controllerName)
 at System.Web.Http.Dispatcher.HttpControllerDispatcher.SendAsyncInternal(HttpRequestMessage request, CancellationToken cancellationToken)
 at System.Web.Http.Dispatcher.HttpControllerDispatcher.SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)


System.ArgumentException

Type 'EST.Controllers.API.TemplatesController' does not have a default constructor


at System.Linq.Expressions.Expression.New(Type type)
 at System.Web.Http.Internal.TypeActivator.Create[TBase](Type instanceType)
 at System.Web.Http.Dispatcher.DefaultHttpControllerActivator.Create(HttpControllerContext controllerContext, Type controllerType)
```



Here’s the actual code:
```csharp
namespace EST.Controllers.API
{
    public class TemplatesController : ApiController
    {
        private readonly ITemplateRepository templateRepository;

        public TemplatesController(ITemplateRepository repository)
        {
            this.templateRepository = repository;
        }

        //
        // GET: /api/Templates/

        public IQueryable Get()
        {
            return templateRepository.All;
        }
    }
}
```
The error message is slightly misleading. The actual error was that I had forgotten to map out my injection via Ninject in /App_Start/NinjectWebCommon.cs
```csharp
private static void RegisterServices(IKernel kernel)
{
    kernel.Bind().To();
}
```
Once I did this the TemplateRepository was able to be instantiated and move forward. Hopefully this helps someone.