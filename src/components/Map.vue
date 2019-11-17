<template>
  <div class="map_area">
    <div>{{ x }}</div>
    <div>{{ y }}</div>
    <div>{{ guard_mode }}</div>
    <img :src="blobUrl1" />
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
      blobUrl1: String,
      blobUrl2: String,
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
        url: "https://webflux-sse-demo.herokuapp.com/map?fileName=map.jpg",
        responseType: "blob"
      }).then(response => {
        this.blobUrl1 = URL.createObjectURL(response.data);
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
    },
    guard_mode: function() {
      if (this.guard_mode === 1) {
        axios({
          method: "GET",
          url: "https://webflux-sse-demo.herokuapp.com/map?fileName=rugby.svg",
          responseType: "blob"
        }).then(response => {
          this.blobUrl2 = URL.createObjectURL(response.data);
        });
      } else if (this.guard_mode === 2) {
        axios({
          method: "GET",
          url:
            "https://webflux-sse-demo.herokuapp.com/map?fileName=triangle.svg",
          responseType: "blob"
        }).then(response => {
          this.blobUrl2 = URL.createObjectURL(response.data);
        });
      }
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
