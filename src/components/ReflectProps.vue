<template>
  <div>
    <div>year: {{ year }}</div>
    <div>month: {{ month }}</div>
    <div>time: {{ time }}</div>
  </div>
</template>

<script>
/* eslint-disable no-console */

export default {
  name: "ReflectProps",

  props: {
    year: Number,
    month: Number,
    time: String
  },
  watch: {
    time: function() {
      console.log(this.props.time);
    }
  }
};
</script>

<style scoped></style>
