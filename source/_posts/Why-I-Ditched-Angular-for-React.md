---
title: Why I Ditched Angular for React
date: 2016-05-27 18:50:59
categories:
    - 前端进阶
    - 外文阅读
tags:
    - Angular
    - React
    - 框架
    - 杂谈
---
{% fullimage http://7xrvo9.com1.z0.glb.clouddn.com/20160527/post-bg-re-vs-ng2.jpg %}

<blockquote class="blockquote-center">A few years ago, when my code started to get cluttered with jQuery selectors and callbacks, AngularJS came to my rescue.

Angular helped me with the maintainability of my dev projects. It came with a lot of functionality out-of-the-box. It was tooled for building large-scale web apps, greatly facilitating rapid development in this genre of applications.</blockquote>

<!--more-->

I remember how its two-way binding and the philosophy of Model as the single source of truth blew me away. And, in practice, they reduced data-redundancy throughout my applications.

Over time though, I discovered some pain points in Angular. Eventually they caused me enough frustration that I began looking around for alternatives.

Here are the concerns I have with Angular.

**DOM for execution.** Angular heavily relies on the DOM for its execution flow. In the default bootstrapping of Angular apps, it scans the DOM and compiles it with priorities of directives, which makes it difficult to debug and test the execution order.

**Two-way binding is a double-edged sword.** As the complexity of your components grows, this approach can lead to performance issues.

How does two-way binding affect performance? JavaScript (ES5) doesn’t have any implementation to notify for any change to its variables or objects, so Angular uses something called "dirty checking" to track data changes and sync them with the user interface (UI).

Dirty-checking is carried out after any operation is performed within Angular’s scope ($digest cycle) which leads to slower performance as the amount of bindings increases.

Another problem with two-way binding is that many components on the page are capable of changing data, which means there are multiple sources of data inputs. If not managed well, this can lead to a confusing and overwhelming situation. To be fair, this is an implementation issue, not an Angular issue in and of itself.

**Angular has its own world.** Every operation in Angular must go through its digest cycle, or else your components won’t sync with your data models. This leads to compatibility issues with other dependencies.

If you use any third-party JavaScript library that involves data changes, you need to wrap it with Angular’s $apply function. Or you will need to convert it to a service, if it’s a utility library. This is like having to reinvent every JavaScript library you use in order for it to interoperate with Angular.

**Dependency injection.** JavaScript currently doesn’t have a package manager and dependency resolver of its own. AMD, UMD and CommonJS have been solving this gap well. But, [until recently](https://github.com/angular/angular.js/issues/4919), Angular did not play well with any of these. Rather, it introduces a dependency injection (DI) of its own. Though, to be fair, there are unofficial Angular dependency-injection implementations using RequireJS.

**Steep learning curve.** Using Angular requires learning a ton of concepts including, but not limited to:
- modules
- controllers
- directives
- scopes
- templating
- linking functions
- filters
- dependency injection

It can be very difficult to get started with Angular. It’s not for the faint of heart.

All of this led me to React.

**What’s So Great About React?**

[React](http://facebook.github.io/react/), the new open source framework for building user interfaces, is a different way of developing JavaScript apps. React is led by Facebook and Instagram.

To be clear: React **isn’t** an application development framework like AngularJS. It’s not fair to compare the two in an apples-to-apples manner.

When React was [introduced](http://2013.jsconf.eu/speakers/pete-hunt-react-rethinking-best-practices.html) at JSConf EU in May 2013, the audience was shocked by some of its concepts, like "one-way data flow" and "Virtual DOM".

React is for building user interfaces. In other words, straight from the official landing page of the project: "people use React as the **V in MVC.**" However, you can write self-contained components with it, which more or less compares to Angular directives.

React rethinks our current web development concepts and best practices.

For instance, it encourages **one-way data flow** and believes in a philosophy that components are state machines driven by data.

Whereas most of the other similar frameworks love working with the DOM and directly manipulating it, React hates the DOM and works to shield the developer from it.

React provides the bare-minimum API needed to define a UI component. Nothing more, nothing less. It follows UNIX philosophy: Small is beautiful. Do one thing, and do it best.

You can find a more detailed [comparison of Angular vs. React](http://www.quora.com/Pete-Hunt/Posts/Facebooks-React-vs-AngularJS-A-Closer-Look) by Pete Hunt (who works at Facebook/Instagram).

**Why Did I Switch to React?**

Here are some of the things I like about React.

**React is Fast**

React takes a different approach to the DOM compared to other frameworks.

It does not let you work with the DOM directly.

It introduces a layer, called **Virtual DOM**, between your JavaScript logic and the actual DOM.

![](http://7xrvo9.com1.z0.glb.clouddn.com/20160527/0485-01-react-virtual-dom.png)

This concept improves web performance. On successive renders, React performs a differential (diff) on the Virtual DOM, and then updates only parts of the actual DOM that need to be updated.

**Cross-Browser Compatibility**

Virtual DOM also helps solve cross-browser issues because it provides us with a standardized API that even works as far back as IE 8.

**Modularity**

Writing self-contained UI components modularizes your app, which in turn isolates issues only to the problematic component/s.

Every component can be developed and tested in isolation, and they can use other components. This equates to maintainability improvements.

**One-way Data Flow Makes Things Saner**

[Flux](http://facebook.github.io/flux/docs/overview.html) is an architecture for creating one-way data layers in JavaScript applications. It was conceptualized by Facebook along with the React view library. The Flux concept makes large-scale app development simpler.

Flux is a concept rather than a tool-specific implementation. It can be incorporated into other frameworks. For instance, Alex Rattray has a nice [implementation of Flux](http://www.toptal.com/front-end/simple-data-flow-in-react-applications-using-flux-and-backbone) using Backbone Collection and Model in React.

**Just JavaScript**

Modern web apps work in a different way compared to the traditional Web.

For example, the View layer needs to be updated with user interactions without hitting the server. And hence View and Controller need to rely on each other heavily.

Many other frameworks use templating engines like Handlebars or Mustache to deal with the View layer. But React believes that View and Controller are so interdependent that they must reside at a single place without the use of any third-party templating engine, and, on top of that, without leaving the scope of JavaScript.

**Isomorphic JavaScript**

The biggest drawback of single-page JS web apps is that it has limitations when crawled by search engines. React has a solution for this.

React can pre-render apps on the server before sending it to the user agent. It can restore the same state into the live application from the pre-rendered static content on the server.

Because search engine crawlers rely on the server response rather than JavaScript execution, pre-rendering your apps helps with SEO.

**It Plays Well with Others**

Loaders and bundlers like RequireJS, Browserify and Webpack are much needed when you’re building large applications. They make the arduous task surmountable.

Unfortunately, the current version of JavaScript doesn’t provide a module bundler or loader. (Though there’s a proposal to address this in the upcoming version, ES6, with System.import).

Fortunately we have some alternatives like RequireJS and Webpack, which are pretty neat. React is built with Browserify, but if you’re looking to inject image assets and compile [Less](http://sixrevisions.com/tutorials/set-up-less-js/) or [CoffeeScript](http://sixrevisions.com/javascript/coffeescript-basics/), then probably Webpack stands as a better option. The point is: You are afforded that choice.

**Do I Need Another Development Framework with React?**

Using React, you can build user interfaces, but you still need to make AJAX calls, apply data filters, and other things that Angular already does.

So if we need an additional JavaScript app development framework, why ditch Angular?

Frameworks are a set of modules and rules. If I don’t need some of its modules, or want to swap out a module for another one that does the job better, how do I do it?

One of the ways to achieve modularity and better dependency-management is through package managers.

But then, how do we manage packages in Angular? That’s up to you, but know that Angular has its own world. You will most likely need to adapt third-party packages into Angular’s world.

React, on the other hand, is just JavaScript. Any package written in JavaScript won’t need any wrapping in React.

For me, using package managers like npm and Bower is better. We can pick and choose our components and craft custom toolsets. To be clear: This is more complicated compared to just using a comprehensive app development framework like Angular.

On this front, the saving grace is that React encourages the use of npm, which has a lot of ready-to-use packages. To get started building apps with React, you can, for example, use one of these [full-stack starter kits](https://github.com/facebook/react/wiki/Complementary-Tools#full-stack-starter-kits).

**Switching to React is Not Painless!**

Since Angular is an app development framework, it comes with a lot of goodies. I’m giving up great features like an AJAX wrapper in the $http service, $q as a promise service, ng-show, ng-hide, ng-class, and ng-if as controlling statements for template — all that amazing stuff.

React isn’t an app development framework, so you need to think about how to deal with the other aspects of building applications. For example, I’m working on an open source project called [react-utils](https://github.com/sahusoftcom/react-utils) which can be used with React to ease development.

The community is also actively contributing similar reusable components to fill in the blanks, so to speak. [React Components](http://react-components.com/) is an unofficial directory website where you can find such open source components.

React’s philosophy does not encourage you to use two-way binding, which brings a lot of pain when you’re dealing with form elements and editable data grids.

However, as you start understanding the Flux data flow and Stores, things become clearer, simpler and easier.

React is new. It will take some time for the community around it to grow. Angular, on the other hand, has already gained huge popularity, and has a relatively large number of extensions available (e.g. AngularUI and Restangular).

However, although React’s community is new, **it is growing fast**. Extensions like React Bootstrap are a testament to this. It’s just a matter of time before we have more components available.

**Conclusion**

If you love the Angular approach, then you may hate React at first. Mainly because of it’s one-way data flow and lack of app development features. You end up needing to take care of many things by yourself.

But as soon as you get comfortable with the Flux design pattern and React’s philosophy, I guarantee that you will begin to see its beauty.

Facebook and Instagram both use React (because they are leading the project).

GitHub’s new source code editor, Atom, is built using React.

The upcoming Yahoo! Mail is being rebuilt in React.

React already has large-scale apps and big tech companies betting on it.

>Author: Kumar Sanket
From: [sixrevisions.com](http://sixrevisions.com/javascript/why-i-ditched-angular-for-react/)



















---
