---
title: 'batman.js - Uncaught TypeError: Cannot call method "dispatch" of undefined'
categories: [Development]
tags: ['CoffeeScript']
---

> I ran in to this error:  
>   
> Uncaught TypeError: Cannot call method ‘dispatch’ of undefined  
>  


The first thing to check is that your route exists. 
I created a utility located here: [https://github.com/RyanonRails/batman.utilitybelt][1] that can list out your current routes.

 [1]: https://github.com/RyanonRails/batman.utilitybelt "https://github.com/RyanonRails/batman.utilitybelt"

If the route exists, you need to make sure that your controller names are named correctly as such:
```coffeescript
@resources 'sections', 'sectionrows'

-------------This would looks for these 2 controllers------------
class EST.SectionsController extends Batman.Controller
class EST.SectionrowsController extends Batman.Controller
```
I ran into this because of the above 2 word model. For future reference here’s the layout I used:

Driver (/est.coffee):
```coffeescript
window.EST = class EST extends Batman.App
  Batman.ViewStore.prefix = 'app/views'
  @controller 'sectionrows'
  @model 'sectionrow'

  @resources 'sectionrows'
```
Controller (/controllers/sectionrows_controller.coffee):
```coffeescript
class EST.SectionrowsController extends Batman.Controller
  routingKey: 'sectionrows'

  index: ->
    EST.SectionRow.load (err,results) =>
      @set 'sectionrows', results
```
Model (/models/sectionrow.coffee):
```coffeescript
class EST.SectionRow extends Batman.Model
  @resourceName: 'sectionrow'
  @url = "/api/sectionrows"

  @set 'primaryKey', 'SectionRowId'
  @persist Batman.RestStorage
  @encode 'SectionRowId', 'WorkType'
```
View (/views/sectionrows/index.html):
```html

<h2>All Section rows</h2>
 
  <div data-foreach-section="sectionrows" data-mixin="animation">
    <a data-bind="sectionrow.SectionRowId" ></a>
    <p data-bind="sectionrow.WorkType"></p>
  </div>
 
Total # of sections: <span data-bind="sectionrows.length"></span>
```
Batman 0.9