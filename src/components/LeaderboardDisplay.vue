<template lang="html">
  <div class="leaderboard">
    <LeaderboardEntry v-for="(entry, index) in leaderboard.data" :key="index" v-bind="entry[0]" />
  </div>
</template>

<script>
import LeaderboardEntry from '@/components/LeaderboardEntry'

export default {
  components: {
    LeaderboardEntry
  },
  props: {
    leaderboard: Object
  }
}
</script>

<style lang="scss" scoped>
.leaderboard {
  display: flex;
  flex-direction: column;
  // align-items: flex-start;
}
</style>
