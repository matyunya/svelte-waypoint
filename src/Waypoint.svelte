<script>
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  export let offset = 0;
  export let throttle = 250;
  export let c = '';
  export let style = '';
  export let once = false;

  let visible = false;

  function throttleFn(fn, threshhold, scope) {
    let last, deferTimer;

    return function () {
      let context = scope || this;

      let now = +new Date,
          args = arguments;
      if (last && now < last + threshhold) {
        // hold on to it
        clearTimeout(deferTimer);
        deferTimer = setTimeout(function () {
          last = now;
          fn.apply(context, args);
        }, threshhold);
      } else {
        last = now;
        fn.apply(context, args);
      }
    };
  }

  function callEvents(wasVisible) {
    if (visible && !wasVisible) {
      dispatch('enter');
    }

    if (!wasVisible && visible) {
      dispatch('leave');
    }
  }

  function waypoint(node) {
    if (!window) return;

    if (visible && once) {
      return;
    }

    if (window.IntersectionObserver && window.IntersectionObserverEntry) {
      const observer = new IntersectionObserver(([ { isIntersecting } ]) => {
        const wasVisible = visible;
        visible = isIntersecting;
        callEvents(wasVisible);
      });

      observer.observe(node);

      return () => observer.destroy();
    }

    function checkIsVisible() {
      if (visible && once) {
        return;
      }

      // Kudos https://github.com/twobin/react-lazyload/blob/master/src/index.jsx#L93
      if (!(node.offsetWidth || node.offsetHeight || node.getClientRects().length)) return;

      let top;
      let height;

      try {
        ({ top, height } = node.getBoundingClientRect());
      } catch (e) {
        ({ top, height } = defaultBoundingClientRect);
      }

      const windowInnerHeight = window.innerHeight
        || document.documentElement.clientHeight;

      const wasVisible = visible;
      visible = (top - offset <= windowInnerHeight) &&
        (top + height + offset >= 0);

      callEvents(wasVisible);
    }

    checkIsVisible();

    const throttled = throttleFn(checkIsVisible, throttle);

    window.addEventListener('scroll', throttled);
    window.addEventListener('resize', throttled);

    const removeHandlers = () => {
      window.removeEventListener('scroll', throttled);
      window.removeEventListener('resize', throttled);
    }

    return removeHandlers;
  }
</script>

<style>
.wrapper {
  display: inline-block;
}
</style>

<div class={`wrapper ${c}`} {style} use:waypoint>
  {#if visible}
    <slot/>
  {:else}
    <slot name="loading"/>
  {/if}
</div>