<template lang="html">
  <div class="query-display">
    <div class="tokens">
      <template v-if="tokens">
        <span class="verb">GET </span>
        <RequestToken v-for="(token, i) in tokens" :key="i" v-bind="token" />
      </template>
      <span v-else>No request sent yet</span>
    </div>
    <span v-if="running">x</span>
    <span v-else-if="tokens">✔</span>
  </div>
</template>

<script>
import RequestToken from '@/components/RequestToken'

export default {
  components: {
    RequestToken
  },
  props: {
    tokens: Array,
    running: Boolean
  }
}
</script>

<style lang="scss" scoped>
.query-display {
  margin: 1em;

  .tokens {
    padding: 1em;
    border-radius: 4px;

    background-color: rgb(93, 93, 93);
    color: white;
  }

  display: flex;
}
</style>
