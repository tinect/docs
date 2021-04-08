# Adding responsive behavior

## Overview
The Shopware 6 Administration provides two ways of adding classes to elements based on their size, the device helper and the `v-responsive` directive.
Alternatively you can use `sccs` media queries to make your plugin responsive.
Learn how to use `scss` here:

{% page-ref page="./add-custom-styles.md" %}

## DeviceHelper

The DeviceHelper provides methods to get device and browser information like the current viewport size.
The helper methods can be accessed with "this.$device" in every Vue component,
since it is bound to the Vue prototype.

It provides functionality to run a function on resize to for example add classes to a element.
The example below shows you how to use the `$device.onResize` helper.

```javascript
const listener = function (ev) {
    // do something on resize with the event, like passing  
};
const scope = this;
const component = 'sw-basic-example';

this.$device.onResize({ listener, scope, component });
this.$device.removeResizeListener(component);
```
It also provides many helper functions e.g. to get the screen dimensions.
Although there are many more as seen below:

| Function                               | Description                 |
|----------------------------------------|-----------------------------|
| `this.$device.getViewportWidth();`     | gets the viewport width     |
| `this.$device.getViewportHeight();`    | gets the viewport height    |
| `this.$device.getDevicePixelRatio();`  | gets the device pixel ratio |
| `this.$device.getScreenWidth();`       | gets the screen width       |
| `this.$device.getScreenHeight();`      | gets screen height          |
| `this.$device.getScreenOrientation();` | gets the screen orientation |


## v-responsive directive

The `v-responsive` directive can be used to dynamically apply classes based on an elements dimensions.

```html
<input v-responsive="{ 'is--compact': el => el.width <= 1620, timeout: 200 }">
```

Let's do a small explanation of this directive:
- Apply class (in this case: `is--compact`) when the width of the element is smaller than 1620px.
- `timeout`: Sets the duration on how much the throttle should wait.
