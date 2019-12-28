# svelte-waypoint
Lazyload images or anything in Svelte and Sapper.

### Install

```
yarn add svelte-waypoint
```
or
```
npm install svelte-waypoint --save
```


### Lazyload your stuff!

```
<script>
  import Waypoint from 'svelte-waypoint';
</script>

/*
  This will check if element is visible every 500ms and will fire
  200 pixels above the element's top border. 
*/
<Waypoint throttle="500" offset="200">
  <MyComponent />
</Waypoint>
```

### Options
- `throttle` *(default: 250)* Interval in milliseconds at which listener function is fired.
- `offset` Distance from the top of the element in pixels at which the element will be marked visible.
- `class` (or `c`, deprecated) Classes passed down to waypoint's wrapper (should be unnecessary once components may receive classes)
- `style` Same as above but for style prop.
- `once` *(default: true)* If false, component will disappear every time it leaves the viewport.
- `threshold` *(default: 0)* "A threshold of 1.0 means that when 100% of the target is visible within the element specified by the root option, the callback is invoked."
- `disabled` *(default: false)* Disable lazyloading.
Currently works only in IE11+.

### Events
- `on:enter` Callback when component enters the viewport.
- `on:leave` Callback when component leaves the viewport.
