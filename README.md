Sane React Apps
===============
A non-exhaustive curated set of guidelines to aid writing sane React applications.

Aims
----
The core aims of these guidelines are to provide sensible patterns and architectural direction on structuring React applications to allow for maintainability, flexibility and scalability - Essentially providing sensible succint patterns to help teams build quickly and safely.

Quick tips, reasonings and examples should all be provided. Further reading too.

What these guidelines are *not*
-------------------------------
These guidelines do not dictate exactly how to write your code but rather provide higher level routines to follow.

Contents
--------

1. [Prop Types](#prop-types)
2. [Pure Render](#pure-render)
3. [Stateless Components](#stateless-components)
4. [Dumb Components](#dumb-components)
5. [Smart Connections](#smart-connections)
6. [Clever Composition](#clever-composition)
7. [Sensible Abstraction](#sensible-abstraction)

## Prop Types
Every Prop used in a component should be documented within the `propTypes` property.

Components should be small and easy-to-use. Two lessons every developer should take from the React library are the productivity wins from 1. a small and clear api and 2. up-front and succinct errors/warnings when things are used incorrectly.

Prop Types allow us to promote these virtues in our own Components by making us think up front about the API we are exposing whilst clearly documenting this api for other consumers.

**Prop Tips:**

1. A Prop Type value is just a function so feel free to create your own to provide even more precise or accurate warnings. Remember the key elements of a good "Prop Type Checker" are a. have succinct error messages and b. are easy and natural to read
2. As components are used side by side with elements it would be more consistent to name props in a similar fashion to element attributes. Eg. `focused` > `isFocused`

## Pure Render
Your `render` method should *always* be pure and have no side effects. This means that given the same `props`, `state` and `context` your `render` function should *always* produce the same result.

Ensuring our render functions are pure means testing is easy (simply mock `props` and `state`) and ensures output is very predictable. It also means that performance-critical optimizations can be made *super* easily via techniques such as basic memoization.

**Render Tips:**

1. The React approach nudges us into functional paradigms which help us split our code up into smaller "easier to reason about" chunks. If your render function appears to be growing in complexity it could be a sign than you should break your component down into smaller components. Alternatively you could simply write re-usable pure functions (not full components) which handle rendering a small portion of your component. *Note: it is actually planned to allow writing of "components" in React as basic pure functions in future versions of React*
2. Having a pure render function means you can now easily implement `shouldComponentUpdate`. Happy days!

## Stateless Components
Local state within components can create a "black box" for data and becomes difficult to maintain as soon as any local state is dependant on outside state (props). The only time local component state should be used is when you are *absolutely* sure that nothing outside of the component would care about that state.

If your components remain stateless you can treat them as pure and garuantee that every time you pass the same props you will render identical output. As mentioned before this means, not only is it incredibly easy to understand and control your output, but also means you can apply easy optimizations and test in a breeze.

Obviously it is not always the most efficient (in terms of developer time) to extract *all* state out of a component into some other form of store but it should be carefully considered whether data should live as local state to a component or out (always better to be out but weigh up the advantages). See below state tips for some helpers on this!

**State Tips:**

1. The only local state you would usually want is UI descriptions such as "is this menu open", "which item in my dropdown is selected" etc. 
2. A nice way to think about local state is this: "If I was to serialize my data and re-render the entire app would I want this data to be correctly restored?" If yes then you should not contain this data as local state but contain it higher up and pass down as props.
3. The higher up your component tree your state the better.
  - *Bonus:* If it can live outside of your component tree nicely then you're onto a winner!

## Dumb Components
*EVERY* component you write for your application can and should be "dumb". This means that the component does not have any concept of *how* to mutate or retrieve your data sources directly but it will simply render using the data provided (via props) and dumbly attempt actions using appropiate provided functions (again via props).

Keeping all your components dumb provides a clear separation of data management and UI render/interaction which will allow for greater maintainability and flexibility down the line and ensures your components are not doing too much work.

Again an additional benefit of this guideline is that testing becomes super easy. (TODO: because...)

**Dumbing Down Tips:**

1. If you can't mock all the functionality of your component from outside via props in a test then it probably isn't dumb enough!

## Smart Connections
So if all my components are dumb how does anything ever *actually* happen then? This is where your "smart" layer needs to come in and provide your application with the brains to the brawn.

Your smart layer could appear in many forms but all have the simple job of providing your dumb components with the data and utilities they need via props.

The recommended form of this would be a "Higher-order Component". In the same fashion that a "Higher-order Function" can take a function and return a new function (where the underlying function can now have access to a higher scope) the Higher-order Component will allow you to provide extra scope (via props) to your dumb component.

**Smart tips:**

1. Export your connected component and the underlying dumb component separately for maximum testability!
2. For a great example of the "dumb"/"smart" theory in play see [react-redux](https://github.com/gaearon/react-redux#dumb-component-is-unaware-of-redux)

## Clever Composition
TBC

## Sensible Abstraction
Don't abstract away too early. React blesses us with an incredible ease to refactor so build quickly and allow patterns to emerge.

---

These should be driven by community-curated best-practices so please feel free to contribute!
