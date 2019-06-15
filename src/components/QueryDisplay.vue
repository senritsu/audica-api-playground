<template lang="html">
  <div class="query-display">
    <div class="tokens">
      <template v-if="tokens">
        <span class="verb">GET </span>
        <RequestToken v-for="(token, i) in tokens" :key="i" v-bind="token" />
      </template>
      <span v-else>No request sent yet</span>
    </div>
    <FontAwesomeIcon :icon="icons.loading" v-if="running" spin size="2x" />
    <FontAwesomeIcon :icon="icons.done" v-else-if="tokens" size="2x" />
  </div>
</template>

<script>
import RequestToken from '@/components/RequestToken'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faSync, faCheck } from '@fortawesome/free-solid-svg-icons'

export default {
  components: {
    RequestToken,
    FontAwesomeIcon
  },
  props: {
    tokens: Array,
    running: Boolean
  },
  computed: {
    icons () {
      return {
        loading: faSync,
        done: faCheck
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.query-display {
  margin: 1em;
  display: flex;
  align-items: center;

  .tokens {
    padding: 1em;
    border-radius: 4px;
    margin-right: 1em;

    background-color: rgb(93, 93, 93);
    color: white;
  }

  display: flex;
}
</style>
