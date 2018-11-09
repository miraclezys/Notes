1. Break the UI into a component hierarchy
2. Build a static version in react
3. Identify the minimal (but complete) representation of UI state
4. Identity where your state should live


One such technique is the single responsibility principle, that is, a component should ideally only do one thing. If it ends up growing, it should be decomposed into smaller subcomponents.

If you’re familiar with the concept of state, don’t use state at all to build this static version. State is reserved only for interactivity, that is, data that changes over time. 

Simply ask three questions about each piece of data:

Is it passed in from a parent via props? If so, it probably isn’t state.
Does it remain unchanged over time? If so, it probably isn’t state.
Can you compute it based on any other state or props in your component? If so, it isn’t state.

 remember that code is read far more than it’s written