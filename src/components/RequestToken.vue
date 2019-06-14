<template lang="html">
  <span class="token">
    <span :class="{[type]: true}">{{ value }}</span>
    <span v-if="type === 'key'" class="separator">=</span>
    <span v-if="type === 'value'" class="separator continuation">&amp;</span>
  </span>
</template>

<script>
export default {
  props: {
    type: String,
    value: {}
  }
}
</script>

<style lang="scss" scoped>
.base, .route, .literal, .separator {
  color: lightgray;
}
.param {
  color: deepskyblue;
}
.key {
  color: orange;
}
.value {
  color: darkorange;
}

.token:last-child > .separator.continuation {
  display: none;
}
</style>
