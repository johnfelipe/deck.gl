<!-- INJECT:"ArcLayerDemo" -->

<p class="badges">
  <img src="https://img.shields.io/badge/64--bit-support-blue.svg?style=flat-square" alt="64-bit" />
</p>

# ArcLayer

The Arc Layer renders raised arcs joining pairs of source and target points,
specified as latitude/longitude coordinates.

```js
import DeckGL, {ArcLayer} from 'deck.gl';

const App = ({data, viewport}) => {

  /**
   * Data format:
   * [
   *   {
   *     inbound: 72633,
   *     outbound: 74735,
   *     from: {
   *       name: '19th St. Oakland (19TH)',
   *       coordinates: [-122.269029, 37.80787]
   *     },
   *     to: {
   *       name: '12th St. Oakland City Center (12TH)',
   *       coordinates: [-122.271604, 37.803664]
   *   },
   *   ...
   * ]
   */
  const layer = new ArcLayer({
    id: 'arc-layer',
    data,
    pickable: true,
    getStrokeWidth: 12,
    getSourcePosition: d => d.from.coordinates,
    getTargetPosition: d => d.to.coordinates,
    getSourceColor: d => [Math.sqrt(d.inbound), 140, 0],
    getTargetColor: d => [Math.sqrt(d.outbound), 140, 0],
    onHover: ({object, x, y}) => {
      const tooltip = `${object.from.name} to ${object.to.name}`;
      /* Update tooltip
         http://deck.gl/#/documentation/developer-guide/adding-interactivity?section=example-display-a-tooltip-for-hovered-object
      */
    }
  });

  return (<DeckGL {...viewport} layers={[layer]} />);
};
```

## Properties

Inherits from all [Base Layer](/docs/api-reference/layer.md) properties.

### Render Options

##### `fp64` (Boolean, optional)

* Default: `false`

Whether the layer should be rendered in high-precision 64-bit mode. Note that since deck.gl v6.1, the default 32-bit projection uses a hybrid mode that matches 64-bit precision with significantly better performance.

##### `widthScale` (number, optional)

* Default: 1

The scaling factor for the width of each arc.

##### `widthMinPixels` (Number, optional)

* Default: 1

The minimum stroke width in pixels.

##### `widthMaxPixels` (Number, optional)

* Default: Number.MAX_SAFE_INTEGER

The maximum stroke width in pixels.

### Data Accessors

##### `getSourcePosition` (Function, optional) ![transition-enabled](https://img.shields.io/badge/transition-enabled-green.svg?style=flat-square")

* Default: `object => object.sourcePosition`

Method called to retrieve the source position of each object.

##### `getTargetPosition` (Function, optional) ![transition-enabled](https://img.shields.io/badge/transition-enabled-green.svg?style=flat-square")

* Default: `object => object.targetPosition`

Method called to retrieve the target position of each object.

##### `getSourceColor` (Function|Array, optional) ![transition-enabled](https://img.shields.io/badge/transition-enabled-green.svg?style=flat-square")

* Default: `[0, 0, 0, 255]`

The rgba color at the source, in `r, g, b, [a]`. Each component is in the 0-255 range.

* If an array is provided, it is used as the source color for all objects.
* If a function is provided, it is called on each object to retrieve its source color.

##### `getTargetColor` (Function|Array, optional) ![transition-enabled](https://img.shields.io/badge/transition-enabled-green.svg?style=flat-square")

* Default `[0, 0, 0, 255]`

The rgba color at the target, in `r, g, b, [a]`. Each component is in the 0-255 range.

* If an array is provided, it is used as the target color for all objects.
* If a function is provided, it is called on each object to retrieve its target color.

##### `getWidth` (Function|Number, optional) ![transition-enabled](https://img.shields.io/badge/transition-enabled-green.svg?style=flat-square")

* Default: `1`

The stroke width of each object, in meters.

* If a number is provided, it is used as the stroke width for all objects.
* If a function is provided, it is called on each object to retrieve its stroke width.

##### `getHeight` (Function|Number, optional) ![transition-enabled](https://img.shields.io/badge/transition-enabled-green.svg?style=flat-square")

* Default: `1`

Multiplier of layer height. `0` will make the layer flat.

##### `getTilt` (Function|Number, optional) ![transition-enabled](https://img.shields.io/badge/transition-enabled-green.svg?style=flat-square")

* Default: `0`

Use to tilt the arc to the side if you have multiple arcs with the same source and target positions.
In degrees, can be positive or negative (`-90 to +90`).

## Source

[/arc-layer](https://github.com/uber/deck.gl/tree/master/modules/layers/src/arc-layer)
