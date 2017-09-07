---
title: Navbar Wizard In Ember
subtitle: 
date: 2017-09-06 14:00 +0000
tags: ember, ember.js, javascript, js, web development, website development, learning, software development
category: Development
author_id: cheryl
image: https://s3.amazonaws.com/kohactive.com/blog/ColorfulLegosJumbled.jpg
banner: true
---

# Navbar Wizard In Ember
Designing intuitive user experiences for non-traditional user flows, such as checkout, can be complicated. In order to solve this, most modern sites provide a wizard to guide the user. The wizard lists breaks the flow into pieces, lists them for the user, and displays their current progress. They help the user understand where they are in the process intuitively.

One such application I recently worked on had a navbar designed with the usual logo on the left, some other text on the right, and something like this in the middle for the first page:

![nav-bar-page-1](https://s3.amazonaws.com/www.kohactive.com/blog/navbar-in-ember/navbar-page1.png)

On the second page, the the left and right were the same but the middle part of the navbar was different:

![nav-bar-page-2](https://s3.amazonaws.com/www.kohactive.com/blog/navbar-in-ember/navbar-page2.png)

The quantity of steps was not certain yet. It might end up being compressed to two steps or possibly being expanded to a fourth optional step before this went into production.

Now we could obviously write this the copy/paste way into a new file for each step `nav-bar-step1`, `nav-bar-step2`, `nav-bar-step3`, but who wants to try to keep that all in sync? We want to make our code more flexible and have fewer files to manage, while at the same time making this first version of the product simple and fast to get started with.

This is a good opportunity to explore component properties in Ember.js. In Ember, a component is a view that is isolated from other views. It has its own data and can be reused in multiple other places. It cannot access properties outside its own scope; properties from the parent view must be explicitly passed in. We're going to build one together for this navigation element.

In this example, we will be passing a static state for the steps in the nav-bar, which will be defined in each call to render the component. You will be able to find additional examples online explaining this passing model properties into the component.

## Version 1 (Using good ol' jQuery)

Hop over to your project directory in the terminal and run the command to generate a component. (Don’t forget it must have a dash in the component name. You must already have a project started and have [Ember-CLI](https://ember-cli.com/) installed.):

```bash
ember generate component nav-bar
```

Call the component to be rendered in the handlebars (`.hbs`) file for the templates where you want to display this component using `{{nav-bar}}`. Open the component’s `.hbs` file and add in your html. We’ll use a `ul` in our example, but you can style this however you like. You just need to define something so you can see a change in the page tell that the value is getting passed.

```html
<ul>
  <li id="step-1"><span class="step">1</span> Information </li>
  <li id="step-2"><span class="step">2</span> Form </li>
  <li id="step-3"><span class="step">3</span> Finish </li>
</ul>
```

In this example, I've added some CSS to distinguish between the three states each step could be in. The default `step` state is upcoming or in the future. For the step they are currently on, I will use a class `step-active`. Likewise, for completed steps I will use the class `step-complete`.

To communicate this to Ember, first we'll be calling for the component to be rendered, then passing it's properties. For the step they are currently on, pass the id `step-2` to the component property `stepActive`. Likewise, for the past steps the user has already completed, pass that id `step-1` to the component property `stepComplete`. Go to the parent template's `.hbs` file and set each of the variables in your call to the component. `{{nav-bar stepActive="step-2" stepComplete="step-1”}}`.

Open the `.js` file for your component. You will need to define variables here that will catch the values we just passed in. We will be defining them in `didInsertElement()` not `init()` because we're using jQuery in this first version of the example. To do this, jQuery must have the component’s html rendered into the DOM to attach properly. More info about these predefined functions at [the Ember.js Guides](https://guides.emberjs.com/v2.14.0/components/the-component-lifecycle/#toc_order-of-lifecycle-hooks-called).

Inside that `didInsertElement()` function, save each property in a variable `var activeStep = this.get('stepActive');`. You could throw in a simple `console.log("activeStep: " + activeStep);` statement next to ensure that everything is connecting properly.

Finally, we want to assign those css classes to the correct DOM elements. I like to check my work with color. Add something to your CSS file like:

```javascript
.step-active {
  background-color: red;
}
```

 Then put in the code to assign the class to the correct element if that property has been passed in to that `didInsertElement()` function.

```javascript
if(activeStep) {
  this.$("#" + activeStep).addClass("step-active");
}
```

Objective complete! See the Twiddle at: [Cheryl's Ember nav-bar twiddle](https://ember-twiddle.com/bf3d6eeb406a3365c860a4bb9e205023?openFiles=templates.components.nav-bar.hbs%2Ctemplates.components.nav-bar.hbs).
More info about Ember Components in the docs: [Ember.Component docs](https://www.emberjs.com/api/ember/2.14.1/classes/Ember.Component).

## Version 2: (Using Ember's Computed Properties)

Now this works... but it's really not leveraging Ember's abilities. It's basically the same as if we wrote some functions at document.ready. This is a good place to start, especially if you are already familiar with jQuery, but let's use this page to show another strategy we could use.

Let's refactor this to use Ember's computed properties. Computed properties in Ember are a great way to allow you to update view data on the fly. They allow you to essentially declare a function that is accessed like a property. Although we will not allow the user to change the value of the properties in this example, computed properties allow your component to react to data changes (such as a model property).

We are going to create a component for each step in our nav-bar. The nav-bar component will call the navbar-step component once for each step and pass in the properties it needs. First, let's create the new component:

```bash
ember generate component navbar-step
```

In `navbar-step.hbs` insert the code for one step.

```html
<li id="step-1"><span class="step">1</span> Information </li>
```

After changing the number and text to be properties and removing the surrounding `li` tags, it becomes:

```html
<span class="step">{{stepNumber}}</span> {{stepLabel}}
```

We won't need that id anymore (we were only using it for targeting). We will need that `li` back rather than the default component tag which is `div` so go to the new component's `navbar-step.js` and set that using `tagName: 'li'`.

Go to your original component, `nav-bar.hbs`, and call each step using the step component. We will need to pass `stepActive` and `stepComplete` through as a property. (There are other ways to handle this if it gets more complex, such as using a service, that won't be covered in this blog post.) I have switched these to just the number instead of the class name.

```html
{{navbar-step stepNumber="1" stepLabel="Information" stepActive=stepActive stepComplete=stepComplete}}`.
```

In `application.hbs` that is:

```html
{{nav-bar stepActive="2" stepComplete="1"}}
```

Here comes the exciting part, the new computed properties. Go to the new component's `navbar-step.js` file and pass the property through and create the new computed properties. (You'll need one for each property: `isStepActive` and `isStepComplete`.)

```javascript
classNameBindings: ['isStepActive:step-active', 'isStepComplete:step-complete'],
isStepActive: Ember.computed('stepActive', 'stepNumber', function(){
  return this.get('stepActive') === this.get('stepNumber')
}),
isStepComplete: Ember.computed('stepComplete', 'stepNumber', function(){
  return this.get('stepComplete') === this.get('stepNumber')
})
```

One last bit, now let's make that checkmark appear than the the step number for completed steps in `navbar-step.hbs`. 

```html
{{#if isStepComplete}}
  &check;
{{else}}
  {{stepNumber}}
{{/if}}
 {{stepLabel}}
```

Now it works like a charm! Hopefully we have also deepened our understanding while building this together.

All done! See the Twiddle at: [Cheryl's Ember nav-bar with computed properties twiddle](https://ember-twiddle.com/4143b09874b7b5028550d5e8f22c0879?openFiles=templates.components.nav-bar.hbs%2Ctemplates.components.navbar-step.hbs).
More info about Ember Computed Properties in the docs: [Ember.Computed docs](https://emberjs.com/api/ember/2.14.1/namespaces/Ember.computed).

