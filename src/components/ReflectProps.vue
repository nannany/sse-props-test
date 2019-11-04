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
  }
};
</script>

<style scoped></style>
