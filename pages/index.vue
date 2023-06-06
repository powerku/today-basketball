<template>
  <main class="flex-shrink-0">
    <!-- Navigation-->
    <nav class="navbar navbar-expand-lg navbar-light bg-white py-3">
      <div class="container px-5">
        <nuxt-link to="/" class="navbar-brand" href="index.html"
          ><span class="fw-bolder text-primary"
            >농구 순번 계산기</span
          ></nuxt-link
        >
      </div>
    </nav>
    <section>
      <div class="gx-5 align-items-center">
        <div class="d-flex justify-content-center mt-xxl-0 flex-column">
          <div class="title">
            <v-btn class="nextButton" icon @click="prevButtonHandler">
              <v-icon>fa-solid fa-angle-left</v-icon>
              이전
            </v-btn>
            <v-subheader class="h2 justify-content-center mt-xxl-0">{{
              quarterInfo
            }}</v-subheader>
            <v-btn class="prevButton" icon @click="nextButtonHandler">
              다음
              <v-icon>fa-solid fa-angle-right</v-icon>
            </v-btn>
          </div>
          <v-simple-table class="justify-content-center mt-xxl-0 d-flex">
            <template #default>
              <thead>
                <tr>
                  <th class="text-left">팀</th>
                  <th class="text-left">뛰는 선수</th>
                  <th class="text-left">휴식 선수</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="item in teams" :key="item.title">
                  <td>{{ item.title }}</td>
                  <td>
                    {{
                      item.players
                        .map((player, index) => {
                          if (player.icon.includes('running')) {
                            return index + 1
                          } else {
                            return null
                          }
                        })
                        .filter((value) => value !== null)
                        .join()
                    }}
                  </td>
                  <td>
                    {{
                      item.players
                        .map((player, index) => {
                          if (!player.icon.includes('running')) {
                            return index + 1
                          } else {
                            return null
                          }
                        })
                        .filter((value) => value !== null)
                        .join()
                    }}
                  </td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
          <div class="match-players-wrapper">
            <MatchPlayers
              v-for="(team, i) in teams"
              :key="i"
              :team="team"
              @add:player="addPlayer(team.players)"
              @remove:player="removePlayer"
              @edit:teamTitle="changeTitle"
            ></MatchPlayers>
            <v-btn
              v-if="teams.length < 3"
              class="btnAddTeam mx-3"
              icon
              @click="addTeamButtonHandler"
            >
              <v-icon>fa-solid fa-plus</v-icon>
              팀추가
            </v-btn>
          </div>
        </div>
      </div>
    </section>
    <!-- Footer-->
    <footer class="bg-white py-4 mt-auto footer">
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
  </main>
</template>

<script>
import MatchPlayers from '../components/match/MatchPlayers.vue'

export default {
  name: 'IndexPage',
  components: { MatchPlayers },
  data: () => ({
    headers: [
      { text: '팀', value: 'team' },
      { text: '1쿼터', value: 'quarter1' },
      { text: '2쿼터', value: 'quarter2' },
      { text: '3쿼터', value: 'quarter3' },
      { text: '4쿼터', value: 'quarter4' },
      { text: '점수', value: 'score' },
    ],
    count: 1,
    teams: [
      {
        startIndex: 1,
        lastIndex: 5,
        title: 'A',
        running: false,
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
      {
        startIndex: 1,
        lastIndex: 5,
        title: 'B',
        running: false,
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
    ],
    teamsLastIndex: 1,
  }),
  computed: {
    quarterInfo() {
      if (this.teams.length < 3) {
        const quarters = ['1쿼터', '2쿼터', '3쿼터', '4쿼터']
        return quarters[(this.count - 1) % 4]
      } else {
        let quarters = ''

        if (this.teamsLastIndex < 4) {
          quarters = '1쿼터'
        } else if (this.teamsLastIndex < 7) {
          quarters = '2쿼터'
        } else if (this.teamsLastIndex < 10) {
          quarters = '3쿼터'
        } else if (this.teamsLastIndex < 13) {
          quarters = '4쿼터'
        }
        const teamInfo = []
        this.teams.forEach((team) => {
          if (team.running) {
            teamInfo.push(team.title)
          }
        })
        return teamInfo.join(' vs ') + ' ' + quarters
      }
    },
  },
  methods: {
    nextButtonHandler() {
      if (this.teams.length > 2) {
        // 3파전
        // 뛴 팀들 다음 차례 사람들
        this.teams.forEach((team) => {
          if (team.running) {
            this.selectNextPlayer(team)
          }
        })

        // 팀 교체
        this.teams.forEach((team) => {
          team.running = false
        })

        for (let i = 0; i < 2; i++) {
          this.teams[
            this.getCircularIndex(this.teams, this.teamsLastIndex + i)
          ].running = true
        }

        this.teamsLastIndex += 1
      } else {
        this.teams.forEach((team) => {
          this.selectNextPlayer(team)
        })
      }

      this.count++
    },
    prevButtonHandler() {
      this.teams.forEach((team) => {
        this.selectPrevPlayer(team)
      })
    },
    addPlayer(players) {
      const index = players.length + 1
      players.push({ text: 'Player' + index, icon: 'fa-solid' })
    },
    removePlayer(players, index) {
      players.splice(index, 1)
    },
    changeTitle(team, teamTitle) {
      team.title = teamTitle
    },
    selectNextPlayer(team) {
      team.players.forEach((player) => {
        player.icon = 'fa-solid'
      })

      for (let i = 0; i < 5; i++) {
        team.players[
          this.getCircularIndex(team.players, team.lastIndex + i)
        ].icon = 'fa-solid fa-person-running'
      }
      team.lastIndex += 5
    },
    selectPrevPlayer(team) {
      team.players.forEach((item) => {
        item.icon = 'fa-solid'
      })
      const startIndex = team.lastIndex - 5
      for (let i = 5; i > 0; i--) {
        team.players[this.getCircularIndex(team.players, startIndex - i)].icon =
          'fa-solid fa-person-running'
      }
      team.lastIndex -= 5
    },
    getCircularIndex(arr, index) {
      const length = arr.length
      return ((index % length) + length) % length
    },
    addTeamButtonHandler() {
      this.teams[0].running = true
      this.teams[1].running = true
      this.teams.push({
        startIndex: 1,
        lastIndex: 5,
        title: 'C',
        running: false,
        players: [
          { text: 'Player1', icon: 'fa-solid fa-person-running' },
          { text: 'Player2', icon: 'fa-solid fa-person-running' },
          { text: 'Player3', icon: 'fa-solid fa-person-running' },
          { text: 'Player4', icon: 'fa-solid fa-person-running' },
          { text: 'Player5', icon: 'fa-solid fa-person-running' },
          { text: 'Player6', icon: 'fa-solid' },
          { text: 'Player7', icon: 'fa-solid' },
        ],
      })
    },
  },
}
</script>

<style scoped>
main {
  height: 100%;
  display: flex;
  flex-direction: column;
}

section {
  flex: 1;
}

.title {
  display: flex;
  justify-content: center;
  align-items: center;
}
.match-players-wrapper {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  margin: 20px;
}

.next-prev-button-wrapper {
  display: flex;
  justify-content: center;
  margin: 10px 20px;
}
.next-prev-button-wrapper button:first-child {
  margin-right: 30px;
}

.btnAddTeam {
  margin-top: auto;
  margin-bottom: auto;
  width: 56px;
  height: 56px;
  border-radius: 50%;
}

.title {
  display: flex;
  flex-direction: row;
  align-items: flex-start;
}
.nextButton {
  height: 48px; !important;
}
.prevButton {
  height: 48px; !important;
}
</style>
