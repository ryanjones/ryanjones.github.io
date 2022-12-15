---
title: MVC3 Ninject/Nunit Unit Tests
categories:
  - Microsoft / CRM / SharePoint / SSRS
---

Here’s some example code for MVC 3 & 4 unit tests. Using Ninject/Moq/Nunit/Repositories
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using NUnit.Framework;
using EST.Controllers;
using EST.Models;
using Moq;
using System.Web.Mvc;

namespace EST.Tests
{
    [TestFixture]
    public class ComplexitiesControllerTest
    {
        private Mock complexityRepoMock;
        private Mock complexityTypeRepoMock;
        private Mock analystTypeRepoMock;
        private ComplexitiesController complexityRepo;

        public AnalystType analystProg;
        public ComplexityType complexityVerySimple;

        [SetUp]
        public void Setup()
        {
            // initial arrange
            complexityRepoMock = new Mock();
            complexityTypeRepoMock = new Mock();
            analystTypeRepoMock = new Mock();
            complexityRepo = new ComplexitiesController(complexityTypeRepoMock.Object, analystTypeRepoMock.Object, complexityRepoMock.Object);

            // Create Analyst Types
            AnalystType analystProg = new AnalystType { Name = "Programmer Analyst" };

            // Create Complexity Types
            ComplexityType complexityVerySimple = new ComplexityType { Name = "Very Simple" };
        }

        [Test]
        public void IndexTest()
        {
            // arrange
            var complexitys = new List
            {
                new Complexity
                {
                    AnalystType = analystProg,
                    ComplexityType = complexityVerySimple,
                    Area = "Design and Architecture",
                    Effort =
                },

                new Complexity
                {
                    AnalystType = analystProg,
                    ComplexityType = complexityVerySimple,
                    Area = "Stored Procedure",
                    Effort =
                },

                new Complexity
                {
                    AnalystType = analystProg,
                    ComplexityType = complexityVerySimple,
                    Area = "Data Access Services",
                    Effort = 0.5
                }
            };

            complexityRepoMock.Setup(a => a.AllIncluding(complexity => complexity.ComplexityType, complexity => complexity.AnalystType)).Returns(complexitys.AsQueryable());

            //act
            var result = complexityRepo.Index() as ViewResult;
            var model = result.ViewData.Model as IQueryable;

            //assert
            Assert.AreEqual(model.Count(), 3);
        }

        [Test]
        public void DetailsTest()
        {
            // arrange
            Complexity complexity = new Complexity()
            {
                ComplexityId = 1,
                AnalystType = analystProg,
                ComplexityType = complexityVerySimple,
                Area = "Design and Architecture",
                Effort =
            };
            complexityRepoMock.Setup(a => a.Find(1)).Returns(complexity);

            // act
            var result = complexityRepo.Details(1) as ViewResult;
            var model = result.ViewData.Model as Complexity;

            // assert
            Assert.AreEqual(model.ComplexityId, 1);
        }

        [Test]
        public void CreateTest()
        {
            // act
            var result = complexityRepo.Create() as ViewResult;

            // assert
            Assert.That(result, Is.Not.Null);
        }

        [Test]
        public void CreatePostTest()
        {
            // arrange
            Complexity complexity = new Complexity()
            {
                ComplexityId = 1,
                AnalystType = analystProg,
                ComplexityType = complexityVerySimple,
                Area = "Design and Architecture",
                Effort =
            };

            var submittedComplexity = new List();
            complexityRepoMock.Setup(z => z.InsertOrUpdate(complexity)).Callback((Complexity a) => submittedComplexity.Add(a));

            //act
            var result = complexityRepo.Create(complexity) as ViewResult;

            // assert
            Assert.AreEqual(submittedComplexity.Count, 1);
        }

        [Test]
        public void CreatePostFailTest()
        {
            // arrange
            Complexity complexity = new Complexity()
            {
                ComplexityId = 1,
                AnalystType = analystProg,
                ComplexityType = complexityVerySimple,
                Area = "Design and Architecture",
                Effort =
            };

            var submittedComplexity = new List();
            complexityRepoMock.Setup(z => z.InsertOrUpdate(complexity));

            //act
            var result = complexityRepo.Create(complexity) as ViewResult;

            // assert
            Assert.AreEqual(submittedComplexity.Count, );
        }

        [Test]
        public void EditTest()
        {
            // arrange
            Complexity complexity = new Complexity()
            {
                ComplexityId = 1,
                AnalystType = analystProg,
                ComplexityType = complexityVerySimple,
                Area = "Design and Architecture",
                Effort =
            };

            complexityRepoMock.Setup(z => z.Find(1)).Returns(complexity);

            //act
            var result = complexityRepo.Edit(1) as ViewResult;
            var model = result.ViewData.Model as Complexity;

            // assert
            Assert.AreEqual(model.ComplexityId, 1);
        }

        [Test]
        public void EditPostTest()
        {
            // arrange
            Complexity complexity = new Complexity()
            {
                ComplexityId = 1,
                AnalystType = analystProg,
                ComplexityType = complexityVerySimple,
                Area = "Design and Architecture",
                Effort =
            };

            complexity.Area = "Design";

            var submittedComplexity = new List();
            complexityRepoMock.Setup(z => z.InsertOrUpdate(complexity)).Callback((Complexity a) => submittedComplexity.Add(a));

            //act
            var result = complexityRepo.Edit(complexity) as ViewResult;

            // assert
            Assert.AreEqual(submittedComplexity.First().Area, "Design");
        }

        [Test]
        public void DeleteTest()
        {
            // act
            var result = complexityRepo.Delete(1) as ViewResult;

            // assert
            Assert.That(result, Is.Not.Null);
        }

        [Test]
        public void DeletePostTest()
        {
            // arrange
            Complexity complexity = new Complexity()
            {
                ComplexityId = 1,
                AnalystType = analystProg,
                ComplexityType = complexityVerySimple,
                Area = "Design and Architecture",
                Effort =
            };

            var submittedComplexity = new List();
            submittedComplexity.Add(complexity);

            complexityRepoMock.Setup(z => z.Delete(1)).Callback(() => submittedComplexity.RemoveAt());

            //act
            var result = complexityRepo.DeleteConfirmed(1) as ViewResult;

            // assert
            Assert.AreEqual(submittedComplexity.Count, );
        }
    }
}
```