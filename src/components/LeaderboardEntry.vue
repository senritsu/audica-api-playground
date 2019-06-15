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
    <div class="score">{{ score }} <span v-if="full_combo" class="fc">FC</span></div>
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
