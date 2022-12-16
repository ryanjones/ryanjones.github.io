---
title: 'batman.js - Data not being submitted to server on create (POST)'
categories: [Development]
tags: ['CoffeeScript']
---


I couldn’t figure out why my data wasn’t being submitted up to the server on a POST (create). Turns out that the data-bind's are case sensitive. IE:
```coffeescript
class EST.Section extends Batman.Model
  @resourceName: 'section'

  @set 'primaryKey', 'SectionId'
  @persist Batman.RestStorage
  @encode 'SectionId', 'Name'
```

They need to match up with @encode ‘SectionId’, ‘Name’

```html
<form id="new_section" data-event-submit="create" data-formfor-section="section">
  <input type="text" placeholder="Section Name" data-bind="section.Name"/>
```

The POST parameters came across once I specified them as case sensitive.

Batman 0.9