<template>
  <div id="app">
    <ReflectProps :year="year" :month="month" :time="time" />
    <div>time{{ time }}</div>
    <button @click="receiveEvent">SSE</button>
  </div>
</template>

<script>
/* eslint-disable no-console,vue/no-shared-component-data */

import ReflectProps from "./components/ReflectProps";
export default {
  name: "app",
  components: {
    ReflectProps
  },
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
      const that = this;
      evtSource.addEventListener(
        "periodic-event",
        function(e) {
          const data = JSON.parse(e.data);
          that.year = data.year;
          that.month = data.month;
          that.time = data.time;
        },
        false
      );
    }
  },
  mounted() {
    this.receiveEvent();
  }
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
