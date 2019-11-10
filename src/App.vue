<template>
  <div id="app">
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
      year: undefined,
      month: undefined,
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
        function(e) {
          const data = JSON.parse(e.data);
          this.time = data.time;
        },
        false
      );
    }
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
