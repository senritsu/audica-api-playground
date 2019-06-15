<template>
  <div id="app">
    <h1>Audica API Playground</h1>

    <QueryDisplay :tokens="lastRequestTokens" :running="running" />

    <ApiKeyInput v-model="apiKey" />

    <main>
      <div class="request-container">
        <RequestRow :step="1" @click="getLeaderboards" :hint="!leaderboards">
          Get leaderboards
        </RequestRow>

        <RequestRow v-if="leaderboards" :step="2" @click="getScores" :hint="!scores">
          Get scores
          <template #settings>
            <div class="score-settings">
              <div class="filters">
                <select name="leaderboard" id="leaderboard" v-model="leaderboard">
                  <option v-for="option in leaderboardOptions" :key="option" :value="option">
                    {{ leaderboards[option].name }}
                  </option>
                </select>

                <select name="platform" id="platform" v-model="platform">
                  <option v-for="option in platformOptions" :key="option" :value="option">
                    {{ leaderboards[leaderboard].platforms[option].name }}
                  </option>
                </select>

                <select name="difficulty" id="difficulty" v-model="difficulty">
                  <option v-for="option in difficultyOptions" :key="option" :value="option">
                    {{ leaderboards[leaderboard].difficulties[option].name }}
                  </option>
                </select>
              </div>

              <div class="pagination">
                <div class="slider">
                  <span>{{ pageSize }} results per page</span>
                  <input type="range" min="1" max="25" v-model="pageSize">
                </div>
                <div class="slider">
                 <span>page {{ page }}</span>
                 <input type="range" min="1" :max="totalPages" v-model="page">
                </div>
              </div>
            </div>
          </template>
        </RequestRow>
      </div>

      <aside class="result">
        <LeaderboardDisplay v-if="scores" :leaderboard="scores.leaderboard" />
      </aside>
    </main>
  </div>
</template>

<script>
import ky from 'ky'

import QueryDisplay from '@/components/QueryDisplay'
import ApiKeyInput from '@/components/ApiKeyInput'
import RequestRow from '@/components/RequestRow'
import LeaderboardDisplay from '@/components/LeaderboardDisplay'

export default {
  name: 'app',
  components: {
    QueryDisplay,
    ApiKeyInput,
    RequestRow,
    LeaderboardDisplay
  },
  data () {
    return {
      apiKey: window.localStorage.getItem('HMX_API_KEY') || '',
      leaderboards: null,
      scores: null,
      leaderboard: 'all_time_leaders',
      difficulty: 'all',
      platform: 'global',
      page: 1,
      pageSize: 10,
      lastRequestTokens: null,
      running: false
    }
  },
  computed: {
    apiUrl () {
      return 'https://audica-api.hmxwebservices.com/public/1/'
    },
    api () {
      return ky.create({
        prefixUrl: this.apiUrl,
        hooks: {
          beforeRequest: [
            req => {
              req.headers.set('Authorization', `X-API-KEY ${this.apiKey}`)
            }
          ]
        }
      })
    },
    leaderboardOptions () {
      return Object.keys(this.leaderboards)
    },
    difficultyOptions () {
      return Object.keys(this.leaderboards[this.leaderboard].difficulties)
    },
    platformOptions () {
      return Object.keys(this.leaderboards[this.leaderboard].platforms)
    },
    totalPages () {
      return this.scores ? this.scores.leaderboard.total_pages : 1
    }
  },
  methods: {
    async getLeaderboards () {
      if (this.running) return

      this.running = true

      this.lastRequestTokens = [
        { type: 'base', value: this.apiUrl },
        { type: 'param', value: 'leaderboards' }
      ]

      this.leaderboards = await this.api.get('leaderboards').json()

      this.running = false
    },
    async getScores () {
      if (this.running) return

      this.running = true

      const { leaderboard, platform, difficulty, page, pageSize } = this

      this.lastRequestTokens = [
        { type: 'base', value: this.apiUrl },
        { type: 'route', value: 'leaderboard' },
        { type: 'literal', value: '/' },
        { type: 'param', value: leaderboard },
        { type: 'literal', value: '?' },
        { type: 'key', value: 'platform' },
        { type: 'value', value: platform },
        { type: 'key', value: 'difficulty' },
        { type: 'value', value: difficulty },
        { type: 'key', value: 'page' },
        { type: 'value', value: page },
        { type: 'key', value: 'page_size' },
        { type: 'value', value: pageSize }
      ]

      this.scores = await this.api.get(`leaderboard/${leaderboard}`, {
        searchParams: {
          platform,
          difficulty,
          page,
          page_size: pageSize
        }
      }).json()

      this.running = false
    }
  },
  watch: {
    apiKey (apiKey) {
      window.localStorage.setItem('HMX_API_KEY', apiKey)
    },
    leaderboard (leaderboard) {
      if (!this.leaderboards[leaderboard][this.platform]) {
        this.platform = Object.keys(this.leaderboards[leaderboard].platforms)[0]
      }
      if (!this.leaderboards[leaderboard][this.difficulty]) {
        this.difficulty = Object.keys(this.leaderboards[leaderboard].difficulties)[0]
      }
    }
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Open+Sans|Open+Sans+Condensed:300&display=swap');

#app {
  font-family: 'Open Sans', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  display: flex;
  flex-direction: column;
  align-items: center;

  * {
    box-sizing: border-box;
  }
}

main {
  margin: 0 10%;
  align-self: stretch;
  display: flex;
  justify-content: space-between;
}

.request-container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;

  width: 800px;
}

.score-settings {
  display: flex;
  flex-direction: column;

  .filters {
    display: flex;
    select {
      font-size: 1.2em;
      margin: 0 0.3em;
    }

    margin-bottom: 1em;
  }

  .pagination {
    display: flex;

    .slider {
      margin: 0.2em 0.5em;
      display: flex;
      flex-direction: column;

      span {
        align-self: flex-start;
      }
    }
  }
}
</style>
