# x5-gmaps ([Live Demo](https://xon52.github.io/x5-gmaps))

![npm bundle size](https://img.shields.io/bundlephobia/minzip/x5-gmaps)

This is a lightweight Google Maps plugin for Vue.

Also check out a [tutorial creating a COVID Heatmap](https://medium.com/javascript-in-plain-english/making-a-covid-map-using-vue-google-maps-89eb70a9f089) using this.

:warning: This plugin is in development, so please let me know if you find any errors.

## Installation

```bash
# npm
npm install x5-gmaps
```

## Deployment

This plugin can be installed like any Vue plugin:

```js
import x5GMaps from 'x5-gmaps'
// Option 1: Just your key
Vue.use(x5GMaps, 'YOUR_GOOGLE_KEY')
// Option 2: With libraries
Vue.use(x5GMaps, { key: 'YOUR_GOOGLE_KEY', libraries: ['places'] })

new Vue({
  el: '#app',
  render: (h) => h(App),
})
```

:warning: This plugin is not transpiled! If you want it compatible with IE, Edge, and Safari, you need to add this to your `vue.config.js` file:

```js
module.exports = {
  transpileDependencies: ['x5-gmaps'],
}
```

<br>

# Usage

```html
<template>
  <gmaps-map>
    <gmaps-marker :position="{ lat: -27, lng: 153 }" />
  </gmaps-map>
</template>
```

```js
import { gmapsMap, gmapsMarker } from 'x5-gmaps'

export default {
  components: { gmapsMap, gmapsMarker },
}
```

<br>

# Provided Components

Some pre-built components have been provided for general use, or as examples for those who wish to take them further.

## Map

![Map](./example/img/readme-map.png)

Maps can take many [options](https://developers.google.com/maps/documentation/javascript/reference/map#MapOptions). `zoom` is defaulted to `12` and `center` is defaulted to Brisbane (as these options are required).

This component supports the following events:

- `@boundsChanged` _returns new bounds_
- `@centerChanged` _returns new center_
- `@click` _returns event_
- `@doubleClick` _returns event_
- `@rightClick` _returns event_
- `@mouseover` _returns event_
- `@mouseout` _returns event_

```html
<template>
  <gmaps-map :options="mapOptions" />
</template>

<script>
  import { gmapsMap } from 'x5-gmaps'

  export default {
    components: { gmapsMap },
    data: () => ({
      mapOptions: {
        center: { lat: -27.47, lng: 153.025 },
        zoom: 12,
      },
    }),
  }
</script>
```

## Marker

![Marker](./example/img/readme-marker.png)

Markers are placed within Maps and can take many [options](https://developers.google.com/maps/documentation/javascript/reference/marker#MarkerOptions). A `position` option is required within the options prop or as its own prop.

This component supports the following events:

- `@move` _returns new position { lat, lng }_
- `@click` _returns event_
- `@doubleClick` _returns event_
- `@rightClick` _returns event_
- ~~`@positionChanged`~~ (depreciated) _returns new position_

| Props     |  Type   | Default | Description                                   |
| :-------- | :-----: | :-----: | :-------------------------------------------- |
| options\* | Object  |    -    | An object of Google Maps Marker options       |
| icon      | String  |    -    | URL of the marker icon to be used             |
| label     | String  |    -    | Marker label                                  |
| opacity   | Number  |  `1.0`  | Opacity of the marker                         |
| position  | Object  |    -    | An object that has `lat` and `lng` properties |
| title     | String  |    -    | Marker title (shown on hover)                 |
| visible   | Boolean | `true`  | If marker is visible                          |
| zIndex    | Number  |    -    | Override position in DOM                      |

_\* If you want to change values on the fly, use the named props instead of within the options prop. Changing named props will trigger an update._

```html
<template>
  <gmaps-map>
    <gmaps-marker v-for="(item, i) in items" :key="i" :options="item.options" />
  </gmaps-map>
</template>

<script>
  import { gmapsMap, gmapsMarker } from 'x5-gmaps'

  export default {
    components: { gmapsMap, gmapsMarker },
    data: () => ({
      items: [
        { options: { position: { lat: -27.41, lng: 153.01 } } },
        { options: { position: { lat: -27.42, lng: 153.02 } } },
        ...,
        { options: { position: { lat: -27.48, lng: 153.08 } } },
        { options: { position: { lat: -27.49, lng: 153.09 } } },
      ],
    }),
  }
</script>
```

## InfoWindow

![InfoWindow](./example/img/readme-info-window.png)

InfoWindows are placed with Maps can take a few [options](https://developers.google.com/maps/documentation/javascript/reference/info-window#InfoWindowOptions). A `position` option is required.

They are used to put HTML in and have a close/dismiss button built-in.

This component only supports a `@closed` event _(for when someone closes the window)_

```html
<template>
  <gmaps-map :options="mapOptions">
    <gmaps-info-window :options="options">
      <p>Example Text</p>
    </gmaps-info-window>
  </gmaps-map>
</template>

<script>
  import { gmapsMap, gmapsInfoWindow } from 'x5-gmaps'

  export default {
    components: { gmapsMap, gmapsInfoWindow },
    data: () => ({
      options: {
        position: { lat: -27.46, lng: 153.02 },
      },
      mapOptions: {
        center: { lat: -27.47, lng: 153.025 },
        zoom: 12,
      },
    }),
  }
</script>
```

## Popup

![Popup](./example/img/readme-popup.png)

A Popup is a custom [DOM Element](https://developers.google.com/maps/documentation/javascript/reference/overlay-view). It is here primarily as an example of what is needed when creating your own map objects, but serves as a cleaner InfoWindow for Vue.

It takes the following props:

- `position` (req'd)
- `background` (style)
- `height` (style)
- `width` (style)

All events are registered from the markup/component you place inside it rather than the popup itself.

```html
<template>
  <gmaps-map :options="mapOptions">
    <gmaps-popup :position="position" background="#BBF0FF">
      <span @click="doSomething()">Do Something</span>
    </gmaps-popup>
  </gmaps-map>
</template>

<script>
  import { gmapsMap, gmapsPopup } from 'x5-gmaps'

  export default {
    components: { gmapsMap, gmapsPopup },
    data: () => ({
      position: { lat: -27.46, lng: 153.02 },
      mapOptions: {
        center: { lat: -27.47, lng: 153.025 },
        zoom: 12,
      },
    }),
  }
</script>
```

## Heatmap

![Heatmap](./example/img/readme-heatmap.png)

Heatmaps are placed within Maps and have several props which are derived from the [GoogleMaps Heatmap Options](https://developers.google.com/maps/documentation/javascript/reference/visualization#HeatmapLayerOptions). Some are named differently as they have been enhanced/simplified.

| Props        |      Type      |   Default    | Description                                                                           |
| :----------- | :------------: | :----------: | :------------------------------------------------------------------------------------ |
| items        | Array\<Object> | **required** | An array of objects that has `lat` and `lng` properties                               |
| colors       | Array\<String> |      -       | An array of one or more colors to color heatmap _e.g. ['red','#0F0','rgba(0,0,0,0)`]_ |
| dissipating  |    Boolean     |    `true`    | Specifies whether heatmaps dissipate on zoom                                          |
| opacity      |     Number     |    `0.6`     | Opacity of the heatmap                                                                |
| maxIntensity |     Number     |      -       | Number of points in one spot to reach "maximum heat" color                            |
| radius       |     Number     |      -       | The radius of influence for each data point, in pixels                                |
| weightProp   |     String     |      -       | The property of items that should be used as the weight (Numbers > 0)                 |

This component does not have any events.

\*\* Note require to include the "visualization" library as described in [Deployment](#deployment)

```html
<template>
  <gmaps-map>
    <gmaps-heatmap :data="items" :opacity="0.8" />
  </gmaps-map>
</template>

<script>
  import { gmapsMap, gmapsHeatmap } from 'x5-gmaps'

  export default {
    components: { gmapsMap, gmapsHeatmap },
    data: () => ({
      items: [
        { lat: -27.41, lng: 153.01 },
        { lat: -27.42, lng: 153.02 },
        ...,
        { lat: -27.48, lng: 153.08 },
        { lat: -27.49, lng: 153.09 },
      ],
    }),
  }
</script>
```

### :warning: **It's highly recommended to check out the demo at the top of this readme to have a play around.**

<!-- TODO: Advanced usage: Component creation, this.$GMaps() etc -->

<br>

---

## Contributing

Please read [CONTRIBUTING.md](./CONTRIBUTING.md) for the process for submitting pull requests.

## Authors

- [Keagan Chisnall](https://github.com/xon52)

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details
