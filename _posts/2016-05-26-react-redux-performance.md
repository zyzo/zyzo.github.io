---
layout: post
title:  "Optimize React-Redux App"
date:   2016-05-26 21:39:00
categories: javascript
comments: true
tags:
- reactjs
- redux
- performance
---

React is advertised as [quite fast out of the box](https://facebook.github.io/react/docs/perf.html). In extreme cases when your app hits performance problem, you just have to optimize the re-rendering process by [shallow comparing](https://facebook.github.io/react/docs/shallow-compare.html) React props and state inside [shouldComponentUpdate](https://facebook.github.io/react/docs/component-specs.html#updating-shouldcomponentupdate) lifecycle function. The strategy for optimizing seems quite straight-forward.

However I found that this strategy is less clear in a React/Redux environment. Here are some advices that you should considering when dealing with your app performance:

### Use Immutable data

This one should be a no-brainer with any React app. `shouldComponentUpdate` can only be useful when all the props can be shallow-compared. So as a rule of thumb, your
components props can only be of 3 types:

- Immutable
- Primitive type (number, boolean, etc.)
- Function type (e.g. callback event)

I didn't mention how you should handle React state. That is because Redux uses a separate store as application state. But sometimes, you need to use both [React's state and Redux's store](https://github.com/reactjs/redux/issues/1287). In that case, both props and state are shallow-compared so state elements should follow mentioned rules as well.

### Do not use ownProps

When you create stateful components (containers) using `connect` function from [react-redux](https://github.com/reactjs/react-redux) library, `shouldComponentUpdate` is declared implicitly, so you normally don't have to implement it yourself.

However the [implementation](https://github.com/reactjs/react-redux/blob/master/src/components/connect.js#L77) of `shouldComponentUpdate` is such that if you uses `ownProps` in your `mapStateToProps` or `mergeProps` function, it will always return false, so the component will always be updated.

This means that if you want your component to be optimized, you can't use information from passed props to get other information from the store. Why the implementation has to be this way is still unclear to me.

### Avoid using `connect` on frequently-called elements

`connect` function wraps the component inside another component with lots of decorated elements. These heavy overheads should be avoided on components that are called a lot in your application, for e.g. an element of a list.

### Use Performance Tools

Finally, here are some excellent tools from the React team. Use them to identify the bottleneck in your React application:

- [React Perf's printWasted() function](https://facebook.github.io/react/docs/perf.html#perf.printwastedmeasurements)
- [React Dev Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en) trace react update
