<template>
  <v-card class="m-5" tile>
    <v-list class="players" dense>
      <v-subheader class="justify-content-center" @click="titleClickHandler"
        >{{ team.title }}
        <v-icon v-if="team.running" class="mx-1">fa-solid fa-basketball</v-icon>
      </v-subheader>
      <v-list-item-group color="primary">
        <v-list-item v-for="(player, i) in team.players" :key="i">
          <v-list-item-content class="index">
            <v-list-item-title>{{ i + 1 }}번 </v-list-item-title>
          </v-list-item-content>
          <v-text-field
            v-model="player.text"
            class="m-0 p-0 name"
            hide-details
            placeholder="이름"
          ></v-text-field>
          <v-list-item-icon>
            <v-icon>{{ player.icon }}</v-icon>
          </v-list-item-icon>
          <v-btn v-if="i > 4" icon x-small @click="removeItem(i)">
            <v-icon>x-small fa-solid fa-minus</v-icon>
          </v-btn>
          <v-btn v-else icon x-small>
            <v-icon>x-small fa-solid</v-icon>
          </v-btn>
        </v-list-item>
        <v-list-item>
          <v-list-item-content>
            <v-list-item-title>선수 추가</v-list-item-title>
          </v-list-item-content>
          <v-btn icon x-small @click="addItem">
            <v-icon>fa-solid fa-plus</v-icon>
          </v-btn>
        </v-list-item>
      </v-list-item-group>
    </v-list>
  </v-card>
</template>

<script>
export default {
  name: 'MatchPlayers',
  props: {
    team: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {
      showEditTitle: false,
    }
  },
  methods: {
    removeItem(index) {
      this.$emit('remove:player', this.team.players, index)
    },
    addItem() {
      this.$emit('add:player')
    },
    editText() {
      this.$emit('edit:player')
    },
    changeTitle(title) {
      this.$emit('edit:teamTitle', this.team, title)
    },
    titleClickHandler() {
      this.showEditTitle = !this.showEditTitle
    },
  },
}
</script>

<style scoped>
.v-list-item__content {
  /*flex: none;*/
  /*margin-right: 5px;*/
}
.name {
  flex: none;
  width: 100px;
  margin-left: 5px;
}
</style>
