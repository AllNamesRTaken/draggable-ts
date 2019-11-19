# draggable-ts

High performance, modern browser, drag and drop in a tiny (2k gzipped), dependency-free package.
Based on bcherny/draggable but rewritten in typescript and without IE8 support.

## Demo

http://allnamesrtaken.github.io/draggable/demos/basic/

## Usage

**HTML**
```html
<div id="id"></div>
```

**JavaScript**

*Using browser globals:*
```js
var element = document.getElementById('id');
var options = {
  grid: 10,
  onDrag: function(){ ... }
};
new Draggable (element, options);
```

*Using AMD/CommonJS:*
```js
var Draggable = require ('Draggable');
var element = document.getElementById('id');
new Draggable (element);
```

## Dependencies

None!

## Options

| Option            | Type                | Default | Description                                                           |
|-------------------|---------------------|---------|-----------------------------------------------------------------------|
| **grid**          | `Number`            | `0`     | grid size for snapping on drag                                        |
| **handle**        | `Element`           | `null`  | the handle of the draggable; if null, the whole element is the handle |
| **filterTarget**  | `Function(target)`  | `null`  | prevent drag when target passes this test                             |
| **limit**         | `Element`, `Function(x, y, x0, y0)`, or `Object` | `{ x: null, y: null }` | limit x/y drag bounds     |
| **threshold**     | `Number`            | `0`     | threshold before drag begins (in px)                                  |
| **setCursor**     | `Boolean` (truthy)  | `false` | change cursor to `move`?                                              |
| **setPosition**   | `Boolean` (truthy)  | `true`  | change draggable position to `absolute`?                              |
| **smoothDrag**    | `Boolean` (truthy)  | `true`  | snap to grid only when dropped, not during drag                       |
| **useGPU**        | `Boolean` (truthy)  | `true`  | move graphics calculation/composition to the GPU? (modern browsers only, graceful degradation) |

## Events

| Event           | Arguments               |
|-----------------|-------------------------|
| **onDrag**      | `element, x, y, event`  |
| **onDragStart** | `element, x, y, event`  |
| **onDragEnd**   | `element, x, y, event`  |

## Instance methods

| Method        | Arguments                               | Returns               | Description
|---------------|-----------------------------------------|-----------------------|-------------------------------------------|
| **get**       | ---                                     | `{Object}` {x, y}     | Get the current coordinates               |
| **set**       | `{Number}` x, `{Number}` y              | instance              | Move to the specified coordinates         |
| **setOption** | `{String}` property, `{Mixed}` value    | instance              | Set an option in the live instance        |
| **destroy**   | ---                                     | ---                   | Unbind the instance's DOM event listeners |

## Notes

Options.limit accepts arguments in several forms:

```js
// no limit
limit: null

// limit x, but leave y unbounded
limit: {
  x: [1,10],
  y: null
}

// limit both axes
limit: {
  x: [1,10],
  y: [1,500]
}

// bound x, set y to a constant
limit: {
  x: [1,10],
  y: 5
}

// bound with an element
limit: document.getElementById('id')

// bound with a custom function
limit: function (
  x,  // current X coordinate
  y,  // current Y coordinate
  x0, // original X coordinate (where drag was started)
  y0  // original Y coordinate (where drag was started)
) {

  var radius = 100,
    dx = x - x0,
    dy = y - y0,
    distance = Math.sqrt(dx*dx + dy*dy),

    // only allow dragging within a circle of radius 100
    outOfRange = distance > radius;


  // if our point is outside of the circle, compute the
  // point on the circle's edge closest to our point
  if (outOfRange) {

    x = x0 + radius * (x - x0) / distance;
    y = y0 + radius * (y - y0) / distance;

  }

  return {
    x: x,
    y: y
  };

}
```

## Tested on

???

## To do

???
