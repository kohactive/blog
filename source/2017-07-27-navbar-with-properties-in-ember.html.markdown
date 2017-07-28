---
title: Navbar With Properties In Ember
subtitle: 
date: 2017-06-27
tags: ember, web development
category: Awards
author_id: cherylschaefer
---

# Navbar With Properties In Ember

It can be easy for a user to get lost in a multi-page process on a web application. One way to help the user understand where they are in a process intuitively is via a navigation element displaying state.

One such application I recently worked on had a navbar designed with the usual logo on the left, some other text on the right, and something like this in the middle for the first page:

![nav-bar-page-1](https://www.dropbox.com/s/h2q3869jwevhlha/navbar-page1.png?dl=0)

On the second page, the the left and right were the same but the middle part of the navbar was different:

![nav-bar-page-2](https://www.dropbox.com/s/x4aqdec7vwceouf/navbar-page2.png?dl=0)

It was pretty clear that the quantity of three steps was not certain. It might end up being compressed to two steps or possibly being expanded to a fourth optional step before this went into production.

Now we could obviously write this the copy/paste way into a new file for each step `nav-bar-step1`, `nav-bar-step2`, `nav-bar-step3`, but who wants to try to keep that all in sync? What if we do need to add a fourth step, or drop down to 2 steps? We want to make our code more flexible and have fewer files to manage, while at the same time making this first version of the product simple and fast to get started with.

This is a good opportunity to explore component properties in Ember.js. In Ember, a component is a view that is isolated from other views. It has its own data and can be reused in multiple other places. It cannot access properties outside its own scope. We're going to build one together for this navigation element.

In this example, we will be passing a static state for the steps in the nav-bar, which will be defined in each call to render the component. You will be able to find additional examples online explaining this passing model properties into the component instead.

Hop over to your project directory in the terminal and run the command to generate a component. (Don’t forget it must have a dash in the component name. You must already have a project started and have [Ember-CLI](https://ember-cli.com/) installed.):

```
ember generate component nav-bar
```

Adjust the handlebars (`.hbs`) file for the templates where you want to display this component using `{{nav-bar}}`. Open the component’s `.hbs` file and add in your html. We’ll use a `ul` in our example, but you can style this however you like. You just need to define something so you can tell that the value is getting passed.

In this example, I've added some CSS to distinguish between the three states each step could be in. The default `step` state is upcoming or in the future. For the step they are currently on, I will use a dynamically added class `step-active`. Likewise, for completed steps I will use the class `step-complete`.

To communicate this to Ember, we need to pass properties over to the component. Go back to your `.hbs` files and set each of the variables in your call to the component. `{{nav-bar stepActive="step-2" stepComplete="step-1”}}`.

First we'll be calling for the component to be rendered, then passing it's properties. For the step they are currently on, pass the id `step-2` to the component property `stepActive`. Likewise, for the past steps the user has already completed, pass that id `step-1` to the component property `stepComplete`.

Open the `.js` file for your component. You will need to define variables here that will be passed to the component’s `.hbs` file. Defining them in `didInsertElement()` not `init()` because we're using jQuery in this example which must have the component’s html rendered into the DOM to attach properly. More info about these predefined functions at [the Ember.js Guides](https://guides.emberjs.com/v2.14.0/components/the-component-lifecycle/#toc_order-of-lifecycle-hooks-called).

Inside that `didInsertElement()` function, save each property in a variable `var activeStep = this.stepActive;`. You could throw in a simple `console.log("activeStep: " + activeStep);` statement next to ensure that everything is connecting property.

Finally, we want to assign those css classes to the correct DOM elements. I like to check my work with color strips rather than scrolling through inspector elements. Add something to your CSS file like:

```
.step-active {
  background-color: red;
}
```
 Then put in the code to assign the class to the correct element if that property has been passed in to that `didInsertElement()` function.
 
 ```
if(activeStep) {
  this.$("#" + activeStep).addClass("step-active");
}
```

Objective complete! See the full Twiddle at: [Cheryl's Ember nav-bar twiddle](https://ember-twiddle.com/bf3d6eeb406a3365c860a4bb9e205023?openFiles=templates.components.nav-bar.hbs%2Ctemplates.components.nav-bar.hbs).
More info about Ember Components in the docs: [Ember.Component docs](https://www.emberjs.com/api/ember/2.14.1/classes/Ember.Component).
