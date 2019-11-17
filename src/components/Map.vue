<template>
  <div class="map_area">
    <div>{{ x }}</div>
    <div>{{ y }}</div>
    <div>{{ guard_mode }}</div>
    <img :src="blobUrl" />
    <img class="place" :style="styleObject" src="/place.svg" />
  </div>
</template>

<script>
/* eslint-disable no-console */

import axios from "axios";

export default {
  name: "Map",
  data: function() {
    return {
      blobUrl: String,
      styleObject: {
        top: "500px",
        left: "500px"
      }
    };
  },
  props: {
    x: undefined,
    y: undefined,
    guard_mode: undefined
  },
  methods: {
    getMap: function() {
      axios({
        method: "GET",
        url: "https://webflux-sse-demo.herokuapp.com/map",
        responseType: "blob"
      }).then(response => {
        this.blobUrl = URL.createObjectURL(response.data);
      });
    },
    updatePlace: () => {
      console.log(this.styleObject);
      this.styleObject.push({ top: this.x + "px", left: this.y + "px" });
    }
  },
  computed: {
    calculateXY: function() {
      console.log("computed" + this.x);
      return this.x + ":" + this.y;
    }
  },
  created() {
    this.getMap();
  },
  mounted() {
    console.log("mounted" + this.x);
    this.updatePlace();
  },
  watch: {
    calculateXY: function() {
      console.log("thisx" + this.x + ":thisy" + this.y);
      this.styleObject = { left: this.x + "px", top: this.y + "px" };
      this.blobUrl = this.x;
    }
  }
};
</script>

<style scoped>
.map_area {
  position: relative;
  border: solid black;
  height: 500px;
  width: 500px;
}
.map_area > .place {
  position: absolute;
  transition: 1s;
}
</style>
