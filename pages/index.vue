<template>
  <div>
    <main class="flex-shrink-0">
      <!-- Navigation-->
      <nav class="navbar navbar-expand-lg navbar-light bg-white py-3">
        <div class="container px-5">
          <nuxt-link to="/" class="navbar-brand" href="index.html"
            ><span class="fw-bolder text-primary"
              >농구 순번 계산기</span
            ></nuxt-link
          >
          <!--          <button-->
          <!--            class="navbar-toggler"-->
          <!--            type="button"-->
          <!--            data-bs-toggle="collapse"-->
          <!--            data-bs-target="#navbarSupportedContent"-->
          <!--            aria-controls="navbarSupportedContent"-->
          <!--            aria-expanded="false"-->
          <!--            aria-label="Toggle navigation"-->
          <!--          >-->
          <!--            <span class="navbar-toggler-icon"></span>-->
          <!--          </button>-->
        </div>
      </nav>
      <!--       Header-->
      <header>
        <div>
          <div class="gx-5 align-items-center">
            <!--            <div class="col-xxl-5">-->
            <!--              &lt;!&ndash; Header text content&ndash;&gt;-->
            <!--              <div class="text-center text-xxl-start">-->
            <!--                <div class="gameCard">-->
            <!--                  <p class="matchUp">-->
            <!--                    <span>송파청소년수련관</span>-->
            <!--                  </p>-->
            <!--                  <div class="GameCardMatchupWrapper">-->
            <!--                    <article class="GameCardMatchup">-->
            <!--                      <div-->
            <!--                        class="MatchupCardTeamName_base__PBkuX"-->
            <!--                        data-team-id="1610612738"-->
            <!--                      >-->
            <!--                        <span class="MatchupCardTeamName_teamName__9YaBA"-->
            <!--                          >White</span-->
            <!--                        >-->
            <!--                      </div>-->
            <!--                    </article>-->
            <!--                    <div class="GameCardMatchup_statusWrapper__TDbQz">-->
            <!--                      <div-->
            <!--                        class="GameCardMatchup_gameCardMatchupStatusWrapper__8rQ8v"-->
            <!--                      >-->
            <!--                        <div-->
            <!--                          class="GameCardMatchupStatusText_gcs__2yfjE"-->
            <!--                          data-is-preseason="false"-->
            <!--                          data-game-status="1"-->
            <!--                        >-->
            <!--                          <p class="GameCardMatchupStatusText_gcsText__PcQUX">-->
            <!--                            2023.05.29 21:00-->
            <!--                          </p>-->
            <!--                        </div>-->
            <!--                      </div>-->
            <!--                    </div>-->
            <!--                    <article class="GameCardMatchup">-->
            <!--                      <div-->
            <!--                        class="MatchupCardTeamName_base__PBkuX"-->
            <!--                        data-team-id="1610612738"-->
            <!--                      >-->
            <!--                        <span class="MatchupCardTeamName_teamName__9YaBA"-->
            <!--                          >Black</span-->
            <!--                        >-->
            <!--                      </div>-->
            <!--                    </article>-->
            <!--                  </div>-->
            <!--                </div>-->
            <!--              </div>-->
            <!--            </div>-->
            <div>
              <!-- Header profile picture-->
              <div class="d-flex justify-content-center mt-xxl-0 flex-column">
                <div class="title">{{ title }}</div>
                <div class="match-players-wrapper">
                  <MatchPlayers
                    :team="teamA"
                    @add:player="addPlayer(teamA.players)"
                    @remove:player="removePlayer"
                  ></MatchPlayers>
                  <MatchPlayers
                    :team="teamB"
                    @add:player="addPlayer(teamB.players)"
                    @remove:player="removePlayer"
                  ></MatchPlayers>
                </div>
                <div class="next-prev-button-wrapper">
                  <v-btn icon @click="prevButtonHandler">
                    <v-icon>fa-solid fa-angle-left</v-icon>
                    이전
                  </v-btn>
                  <v-btn icon @click="nextButtonHandler">
                    다음
                    <v-icon>fa-solid fa-angle-right</v-icon>
                  </v-btn>
                </div>
              </div>
            </div>
          </div>
        </div>
      </header>
    </main>
    <!-- Footer-->
    <footer class="bg-white py-4 mt-auto">
      <div class="container px-5">
        <div
          class="row align-items-center justify-content-between flex-column flex-sm-row"
        >
          <div class="col-auto">
            <div class="small m-0">Copyright &copy; powerku 2023</div>
          </div>
          <div class="col-auto">
            <!--            <a class="small" href="#!">Privacy</a>-->
            <!--            <span class="mx-1">&middot;</span>-->
            <!--            <a class="small" href="#!">Terms</a>-->
            <!--            <span class="mx-1">&middot;</span>-->
            <a class="small" target="_blank" href="https://powerku.tistory.com/"
              >Contact</a
            >
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
import MatchPlayers from '../components/match/MatchPlayers.vue'

export default {
  name: 'IndexPage',
  components: { MatchPlayers },
  data: () => ({
    count: 1,
    teamA: {
      startIndex: 1,
      lastIndex: 5,
      title: 'Black',
      players: [
        { text: 'Player1', icon: 'fa-solid fa-person-running' },
        { text: 'Player2', icon: 'fa-solid fa-person-running' },
        { text: 'Player3', icon: 'fa-solid fa-person-running' },
        { text: 'Player4', icon: 'fa-solid fa-person-running' },
        { text: 'Player5', icon: 'fa-solid fa-person-running' },
        { text: 'Player6', icon: 'fa-solid' },
        { text: 'Player7', icon: 'fa-solid' },
      ],
    },
    teamB: {
      startIndex: 1,
      lastIndex: 5,
      title: 'White',
      players: [
        { text: 'Player1', icon: 'fa-solid fa-person-running' },
        { text: 'Player2', icon: 'fa-solid fa-person-running' },
        { text: 'Player3', icon: 'fa-solid fa-person-running' },
        { text: 'Player4', icon: 'fa-solid fa-person-running' },
        { text: 'Player5', icon: 'fa-solid fa-person-running' },
        { text: 'Player6', icon: 'fa-solid' },
        { text: 'Player7', icon: 'fa-solid' },
      ],
    },
  }),
  computed: {
    title() {
      const quarters = ['1쿼터', '2쿼터', '3쿼터', '4쿼터']
      return quarters[(this.count - 1) % 4]
    },
  },
  methods: {
    nextButtonHandler() {
      this.selectNextPlayer(this.teamA)
      this.selectNextPlayer(this.teamB)
      this.count++
    },
    prevButtonHandler() {
      this.selectPrevPlayer(this.teamA)
      this.selectPrevPlayer(this.teamB)
    },
    addPlayer(players) {
      const index = players.length + 1
      players.push({ text: 'Player' + index, icon: 'fa-solid' })
    },
    removePlayer(players, index) {
      players.splice(index, 1)
    },
    selectNextPlayer(team) {
      team.players.forEach((player) => {
        player.icon = 'fa-solid'
      })

      for (let i = 0; i < 5; i++) {
        team.players[this.getCircularIndex(team, team.lastIndex + i)].icon =
          'fa-solid fa-person-running'
      }
      team.lastIndex += 5
    },
    selectPrevPlayer(team) {
      team.players.forEach((item) => {
        item.icon = 'fa-solid'
      })
      const startIndex = team.lastIndex - 5
      for (let i = 5; i > 0; i--) {
        team.players[this.getCircularIndex(team, startIndex - i)].icon =
          'fa-solid fa-person-running'
      }
      team.lastIndex -= 5
    },
    getCircularIndex(team, index) {
      const length = team.players.length

      return ((index % length) + length) % length
    },
  },
}
</script>

<style scoped>
.title {
  display: flex;
  justify-content: center;
}
.match-players-wrapper {
  display: flex;
  justify-content: center;
  margin: 0 20px;
}

.next-prev-button-wrapper {
  display: flex;
  justify-content: center;
  margin: 10px 20px;
}
.next-prev-button-wrapper button:first-child {
  margin-right: 30px;
}
</style>
