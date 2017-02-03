---
layout: post
title: "Introduction to Ember Closure Actions"
date: 2017-01-20 15:07:40 +0100
comments: true
author: Jovica Susa
categories:
  - Ember
tags:
  - ember
  - javascript
published: true
---

Closure actions were introduced in Ember v.1.13.0 and they brought a lot of improvements over old action handling mechanism in Ember. These improvements enabled Ember to adopt new data flow model called - Data Down Actions Up (DDAU) that simplified communication between parent and child components.

## What are closure acitons?

Closure actions are based on JavaScript closures which are basically functions that remember environment in which they were created. So closure actions are just functions that remember context in which they were defined. Since they are just functions we can pass them as a value and call them directly as a callback. This enables us to pass them to inner components and call them directly from components.
With the old approach we had to use `sendAction()` from component and call action on controller or route.
## How to create closure actions?

Every action defined in controller can become a closure action. It’s important to note that it’s possible to make actions defined in routes closure actions but you need to use [ember-route-action-helper](https://github.com/DockYard/ember-route-action-helper) addon for that. Closure actions are created in template using `action` helper which wrapps the action in the current context and returns it as a closure function.

## Example

Let’s define action inside our application controller.
```
//controllers/application.js
export default Ember.Controller.extend({
  actions: {
    submit() {
      //some logic
    }
  }
});
```
In application.hbs we create closure action using `action` helper and we assign it to example-comp's save attribute.

```
{% raw %}
{{example-comp save=(action 'submit')}}
{% endraw %}
```

Submit action is now assigned to save attribute of example-comp component so we can call it directly inside component.

```
	<!-- example-comp.hbs -->
{% raw %}
<button onclick={{action 'saveItem'}}>Save</button>
{% endraw %}
```
```
//example-comp.js
export default Ember.Component.extend({
  actions: {
    saveItem() {
      this.save();
      // Calling save directly
    }
  }
});
```

Passing closure action through multiple levels of components is easy.
Let's say we want to call submit action in example-comp-child component. Since we have save attribute inside example-comp with submit action assigned to it we just need to pass it one level down.
```
{% raw %}
{{example-comp-child save=(action save)}}
{% endraw %}
```

Calling action from example-comp-child is the same as for example-comp component.

```
//example-comp-child.js
export default Ember.Component.extend({
  actions: {
    saveItem() {
      this.save();
    }
  }
});
```

Closure actions simplify action passing mechanism in Ember but they can also return values, enable currying and much more. It definitely worth spending your time learning all closure actions features and I hope this introduction can help with that.
