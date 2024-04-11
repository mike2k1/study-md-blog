---
title: "Reconciliation in React"
subtitle: "My little understanding of Reconciliation and related problems"
date: "2024-04-10"
---

## DOM
DOM = Document Object Modal.

`DOM` represents the interface of the application. If there is any change in UI of the application, `DOM` is updated to show the change. If `DOM` is mutated frequently, the performance is bad especially if we have a lot of elements in user interface.

## Virtual DOM
### Why Virtual DOM is faster?
When new element is added to UI, a `virtual DOM` is created. Each element is a node in the tree. If these elements have changes in state/props => Create new `virtual DOM` then "diffing" with prev `virtual DOM` and detect where changes are needed.

![images/Diffing](/images/diffing.png)

## Diff algorithm (Reconciliation)
When comparing 2 virtual DOM, `React` algorithm decides if we can reuse `Element` or recreate `Element`.

### Elements of different types
Whenever the root elements have different types, `React` will remove old tree and build new tree (full rebuild). Any components below the root will also get unmounted and have their state destroyed. 

```bash
<div>
  <Counter />
</div>

<span>
  <Counter />
</span>
```
### Elements of same types


```bash
<button className="blue" />
{
    type: 'button',
    props: { className: 'blue' }
}

<button className="red" />
{
    type: 'button',
    props: { className: 'red' }
}
```

Only `className` is changed => No need to  create `<button>`, only update `className`.

```bash
<div style={{color: 'red', fontWeight: 'bold'}} />

<div style={{color: 'green', fontWeight: 'bold'}} />
```

### Recursing On Children
By default, when recursing on the children of a DOM node, React just iterates over both lists of children at the same time and generates a mutation whenever there’s a difference.

When adding an element at the end of the children, converting between these two trees works well:

```bash
<ul>
  <li>first</li>
  <li>second</li>
</ul>

<ul>
  <li>first</li>
  <li>second</li>
  <li>third</li>
</ul>
```

`React` will match the two `<li>first</li>` trees, match the two `<li>second</li>` trees, and then insert the `<li>third</li>` tree.

However, if adding an element at the top,

```bash
<ul>
  <li>first</li>
  <li>second</li>
</ul>

<ul>
  <li>third</li>
  <li>first</li>
  <li>second</li>
</ul>
```

`React` will mutate every child instead of keeping `<li>first</li>` and `<li>second</li>` => Performance problem => Keys

### Keys

When children have keys, `React` uses key to match children in old tree vs new tree.

Item's index can be used as a key => Reorders will make issues.