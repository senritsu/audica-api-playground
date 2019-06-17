<template lang="html">
  <div class="leaderboard-entry row">
    <div class="rank">{{ rank }}</div>
    <div class="user col">
      <div class="info row">
        <FontAwesomeIcon v-if="developer" :icon="icons.dev"/>
        <span class="name">{{ user }}</span>
      </div>
      <span class="platform">{{ platform }}</span>
    </div>
    <div class="score">
      <FontAwesomeIcon v-if="full_combo" :icon="icons.fc"/>
      {{ score }}
    </div>
  </div>
</template>

<script>
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faStar, faCode } from '@fortawesome/free-solid-svg-icons'

export default {
  components: {
    FontAwesomeIcon
  },
  props: {
    developer: Boolean,
    full_combo: Boolean,
    platform: String,
    rank: Number,
    score: Number,
    user: String
  },
  computed: {
    icons () {
      return {
        dev: faCode,
        fc: faStar
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.row {
  display: flex;
  align-items: center;
}

.col {
  display: flex;
  flex-direction: column;
}

.leaderboard-entry {
  display: flex;
  align-items: center;
  justify-content: flex-start;

  &:not(:last-child) {
    margin-bottom: 0.4em;
  }
}

.rank {
  font-size: 2em;
  width: 2em;
  text-align: right;
  margin-right: 0.25em;
}

.user {
  font-size: 1.3em;
  align-items: flex-start;
  margin-right: 1em;

  .platform {
    font-size: 0.7em;
    color: gray;
  }
}

.score {
  font-family: 'Open Sans Condensed', sans-serif;
  font-size: 2em;
  font-weight: lighter;
  margin-left: auto;
  color: darkorange;
}
</style>
