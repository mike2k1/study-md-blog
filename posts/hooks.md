---
title: "Hooks"
subtitle: "My little understanding of Hooks and related problems"
date: "2024-04-10"
---

## useEffect vs useLayoutEffect

### Browsers paint the screen
When visit a web page

#### HTML Parsing:
Parsing the HTML document received from the server and create a DOM tree
#### CSS Parsing and Styling:
Parsing the CSS stylesheets => CSSOM + DOM => Render tree
#### Layout:
Calculating the layout (size, position, relationship) of each element
#### Painting:


### useEffect
The callback is executed after the browser paints the screen and users see the change in UI

### useLayoutEffect
The callback is executed before the browser paints the screen => Use when need to access the updated DOM before painted

## Ref, useRef, createRef

### Refs
`React ref(reference)` is a reference to a variable, component that keeps the value unchanged between `re-renders` and accesses value through `current`.

Ref and state are retained between `re-renders` => Changing a state make `re-renders`. Ref is not.

Ref is used to access a DOM element.

```
<div ref={myRef}>
```

`React` puts the DOM element into `myRef.current` => Element is removed from DOM, `myRef.current` is `null`.

### useRef
`useRef` return an object with `current` property. 