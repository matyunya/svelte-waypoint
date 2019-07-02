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
- `throttle` *(default: 250)* — Interval in milliseconds at which listener function is fired.
- `offset` *(0)* — Distance from the top of the element in pixels at which the element will be marked visible.
- `c` *''* — Classes passed down to waypoint's wrapper (should be unnecessary once components may receive classes)
- `style` *''* — Same as above but for style prop.