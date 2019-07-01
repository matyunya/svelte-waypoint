# svelte-waypoint
Lazyload images or anything component for Svelte as simple as

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
  <MyComponent/>
</Waypoint>
```

