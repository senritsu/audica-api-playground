<template lang="html">
  <div>
    <strong>{{ rank }}</strong> {{ user }} {{ score }}
  </div>
</template>

<script>
export default {
  props: {
    developer: Boolean,
    full_combo: Boolean,
    platform: String,
    rank: Number,
    score: Number,
    user: String
  }
}
</script>

<style lang="scss" scoped>
</style>
