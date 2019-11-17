<template>
  <div>
    <div>time:{{ time }}</div>
    <button @click="receiveEvent">SSE</button>
    <div>{{ x }}</div>
    <Map :x="this.x" :y="this.y" :guard_mode="this.guard_mode" />
  </div>
</template>

<script>
/* eslint-disable no-console,vue/no-shared-component-data */

import Map from "./components/Map";
export default {
  name: "app",
  components: { Map },
  data: function() {
    return {
      time: undefined,
      x: Number,
      y: Number,
      guard_mode: Number
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
    },
    receivePlaceEvent: function() {
      const evtSource = new EventSource(
        "https://webflux-sse-demo.herokuapp.com/notification/events"
      );
      evtSource.addEventListener(
        "receive/roboticbase-event",
        e => {
          const data = JSON.parse(e.data);
          this.x = data.x;
          this.y = data.y;
          this.guard_mode = data.guard_mode;
        },
        false
      );
    }
  },
  created() {
    this.receivePlaceEvent();
  }
};
</script>
