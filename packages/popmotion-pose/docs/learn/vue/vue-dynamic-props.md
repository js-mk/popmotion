---
title: Dynamic pose props
description: Use props in Pose for Vue to create dynamic poses.
category: vue
next: vue-animating-children
---

# Dynamic pose props

Each pose property can be set as a function that resolves when the pose is entered:

```javascript
const Box = posed.div({
  visible: {
    x: 0,
    y: (props) => 100, // Resolved on `visible` enter
    transition: {
      x: { type: 'tween' },
      y: (props) => ({ type: 'spring' }) // Resolved on `visible` enter
    }
  }
})
```

By using the provided `props` argument, this allows us to create dynamic properties that will react to changes in your app.

<TOC />

## Props

So what are props? In Pose for Vue, they're any taken from any [non-prop attributes](https://vuejs.org/v2/guide/components-props.html#Non-Prop-Attributes) set on the posed component.

For instance, you could use a component's `i` index attribute to write a dynamic `delay` prop:

```javascript
const Component = {
  components: {
    Item: posed.li({
      visible: {
        opacity: 1,
        transition: ({ i }) => ({ delay: i * 50 })
      }
    })
  },
  template: `<ul>
  <Item
    :pose="isVisible ? 'visible' : 'hidden'"
    v-for="item in items"
    v-bind:key="item"
    v-bind:i="item"
  />
</ul>`
};
```

<CodeSandbox id="v1vx1y2yz0" height="500" vue />

## Transition props

`transition` works a little differently than other pose props.

If set as a function, the function is run **once each for every property being animated**.

That function is provided a few extra props, automatically generated by Pose:

- `from`: The current state of this value
- `to`: The target state defined in the pose
- `velocity`: If a numerical value, the current velocity of the value
- `key`: The name of the current value
- `prevPoseKey`: The name of the pose this value was previously set to

These props can be used to return a different transition definition based on the state of the value:

```javascript
const Sidebar = posed.div({
  open: {
    x: '-100%',
    transition: ({ velocity, to }) => velocity < 0
      ? { to: 0 }
      : { to }
  }
});
```

If `transition` is a named map, **some or all** of these can be defined as functions:

```javascript
const Sidebar = posed.div({
  open: {
    x: 0,
    opacity: 1,
    transition: {
      x: ({ velocity, to }) => velocity < 0 ? { to: -300 } : { to },
      opacity: { type: 'spring' }
    }
  }
});
```
