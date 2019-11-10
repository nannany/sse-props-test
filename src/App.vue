<template>
  <div>
    <div>time:{{ time }}</div>
    <button @click="receiveEvent">SSE</button>
  </div>
</template>

<script>
/* eslint-disable no-console,vue/no-shared-component-data */

export default {
  name: "app",
  data: function() {
    return {
      time: undefined
    };
  },
  methods: {
    receiveEvent: function() {
      const evtSource = new EventSource(
        "https://webflux-sse-demo.herokuapp.com/stream-sse"
      );
      evtSource.addEventListener(
        "periodic-event",
        e => {
          const data = JSON.parse(e.data);
          this.time = data.time;
        },
        false
      );
    }
  }
};
</script>
