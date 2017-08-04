---
title: Navbar With Properties In Ember
subtitle: 
date: 2017-08-08
tags: ember, ember.js, javascript, js, web development, website development, learning, software development
category: Development
author_id: cheryl
---

# Navbar With Properties In Ember
It can be easy for a user to get lost in a multi-page process on a web application. One way to help the user understand where they are in a process intuitively is via a navigation element displaying state.

One such application I recently worked on had a navbar designed with the usual logo on the left, some other text on the right, and something like this in the middle for the first page:

![nav-bar-page-1](https://s3.amazonaws.com/www.kohactive.com/blog/navbar-in-ember/navbar-page1.png)

On the second page, the the left and right were the same, but the middle part of the navbar was different:

![nav-bar-page-2](https://s3.amazonaws.com/www.kohactive.com/blog/navbar-in-ember/navbar-page2.png)

It was pretty clear that the quantity of three steps was not certain. It might end up being compressed to two steps or possibly being expanded to a fourth optional step before this went into production.

WNow we could obviously write this the copy/paste way into a new file for each step `nav-bar-step1`, `nav-bar-step2`, `nav-bar-step3`, but who wants to try to keep that all in sync? What if we do need to add a fourth step, or drop down to 2 steps? We want to make our code more flexible and have fewer files to manage, while at the same time making this first version of the product simple and fast to get started with.

This is a good opportunity to explore component properties in Ember.js. In Ember, a component is a view that is isolated from other views. It has its own data and can be reused in multiple other places. It cannot access properties outside its own scope. We're going to build one together for this navigation element.

In this example, we will be passing a static state for eachthe step components in the nav-bar, which will be defined in each call to render the component. You will be able to find additional examples online explaining this passing model properties into the component if you wanted to dynamically create componentsinstead.

## Version 1: Using jQuery

Hop over to your project directory in the terminal and run the command to generate a component. (Don’t forget it must have a hyphendash in the component name. You must already have a project started and have [Ember-CLI](https://ember-cli.com/) installed.):

```
ember generate component nav-bar
```

Add a `{{nav-bar}}`Adjust the to your handlebars (`.hbs`) file for the templates where you want to display this component using `{{nav-bar}}`. Open the component’s `.hbs` file and add in your HTMLhtml. We’ll use a `ul` in our example, but you can style this however you’d like. You just need to define something so you can tell that the value is getting passed.

In this example, I've added some CSS to distinguish between the three states each step could be in. The default `step` state is upcoming or in the future. For the step the user isy are currently on, I will use a class `step-active`. Likewise, for completed steps, I will use the class `step-complete`.

To communicate this to Ember, we need to pass properties over to the component. Go back to your `.hbs` files and set each of the variables in your call to the component. `{{nav-bar stepActive="step-2" stepComplete="step-1”}}`.

First we'll be calling for the component to be rendered, then passing it's properties. For the step the user isy are currently on, pass the id `step-2` to the component property `stepActive`. Likewise, for the past steps the user has already completed, pass that id `step-1` to the component property `stepComplete`.

Open the `.js` file for your component. You will need to define variables here that will be passed to the component’s `.hbs` file. Defineing them in `didInsertElement()` not `init()` because we're using jQuery in this first version of the example. To do this, jQuery must have the component’s HTMLhtml rendered into the DOM to attach properly. More info about these predefined functions at [the Ember.js Guides](https://guides.emberjs.com/v2.14.0/components/the-component-lifecycle/#toc_order-of-lifecycle-hooks-called).

Inside theat `didInsertElement()` function, save each property in a variable `var activeStep = this.get('stepActive');`. You could throw in a simple `console.log("activeStep: " + activeStep);` statement next to ensure that everything is connecting properlty.

Finally, we want to assign those CSScss classes to the correct DOM elements. I like to check my work with color strips rather than scrolling through inspector elements. Add something to your CSS file like:

```
.step-active {
  background-color: red;
}
```

 Then put in the code to assign the class to the correct element if that property has been passed in to theat `didInsertElement()` function.

```
if(activeStep) {
  this.$("#" + activeStep).addClass("step-active");
}
```

Objective complete! See the Twiddle at: [Cheryl's Ember nav-bar twiddle](https://ember-twiddle.com/bf3d6eeb406a3365c860a4bb9e205023?openFiles=templates.components.nav-bar.hbs%2Ctemplates.components.nav-bar.hbs).
More info about Ember Components in the docs: [Ember.Component docs](https://www.emberjs.com/api/ember/2.14.1/classes/Ember.Component).

## Version 2: Using Ember's Computed Properties

Now this works... but it's really not leveraging Ember's abilities. It's basically the same as if we wrote some functions at document.ready. Doing anything more may be excessive in this case, but let's use this page to show another strategy we could use.

Let's refactor this to use Ember's computed properties. Computed properties in Ember are a great way to allow you to update view data on the fly. They allow you to essentially declare a function that is accessed like a property.

We are going to create a component for each step in our nav-bar. The nav-bar will call the navbar-step component for each step and pass in the properties it needs. First, let's create the new component:

```
ember generate component navbar-step
```

In `navbar-step.hbs` insert the code for one step.

```
<li id="step-1"><span class="step">1</span> Information </li>
```

After changing the number and text to be properties and removing the surrounding `li` tags, it becomes:

```
<span class="step">{{stepNumber}}</span> {{stepLabel}}
```

We won't need that id anymore (we were only using it for targeting). We will need that `li` back rather than the default component tag which is `div,` so go to the new component's `navbar-step.js` and set that using `tagName: 'li'`.

Go to your original component, `nav-bar.hbs`, and call each step using the step component. We will need to pass `stepActive` and `stepComplete` through as a propertiesy. (There are other ways to handle this if it gets more complex, such as using a service, but that won't be covered in this blog post.) I have switched these to just the number instead of the class name.

```
{{navbar-step stepNumber="1" stepLabel="Information" stepActive=stepActive stepComplete=stepComplete}}`.
```

In `application.hbs` that is:

```
{{nav-bar stepActive="2" stepComplete="1"}}
```

Here comes the exciting part, the new computed properties. Go to the new component's `navbar-step.js` file and pass the property through and create the new computed properties. (You'll need one for each property.)

```
classNameBindings: ['isStepActive:step-active'],
isStepActive: Ember.computed('stepActive', 'stepNumber', function(){
  return this.get('stepActive') === this.get('stepNumber')
})
```

One last bit, now let's make that checkmark appear rather than the the step number for completed steps in `navbar-step.hbs`. 
```
{{#if isStepComplete}}
  &check;
{{else}}
  {{stepNumber}}
{{/if}}
 {{stepLabel}}
```

Now it works like we expect it to again. Although it takes more files/lines of code in this simple example, computed properties allow for simple control in more complex cases.

All done! See the Twiddle at: [Cheryl's Ember nav-bar with computed properties twiddle](https://ember-twiddle.com/4143b09874b7b5028550d5e8f22c0879?openFiles=templates.components.nav-bar.hbs%2Ctemplates.components.navbar-step.hbs).
More info about Ember Computed Properties in the docs: [Ember.Computed docs](https://emberjs.com/api/ember/2.14.1/namespaces/Ember.computed).
