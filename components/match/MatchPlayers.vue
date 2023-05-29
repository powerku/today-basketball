<template>
  <v-card class="mx-auto" tile>
    <v-list class="players" dense>
      <v-subheader class="justify-content-center">{{ team.title }}</v-subheader>
      <v-list-item-group color="primary">
        <v-list-item v-for="(player, i) in team.players" :key="i">
          <v-list-item-content class="index">
            <v-list-item-title>{{ i + 1 }}번 </v-list-item-title>
          </v-list-item-content>
          <!--          <v-list-item-content>-->
          <!--            <v-list-item-title>{{ player.text }}</v-list-item-title>-->
          <!--          </v-list-item-content>-->
          <v-text-field
            v-model="player.text"
            class="m-0 p-0"
            hide-details
            placeholder="이름"
          ></v-text-field>
          <v-list-item-icon>
            <v-icon disabled>{{ player.icon }}</v-icon>
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
  methods: {
    removeItem(index) {
      this.$emit('remove:player', this.team.players, index)
    },
    addItem() {
      this.$emit('add:player')
    },
    editText() {
      console.log('edit')
      this.$emit('edit:player')
    },
  },
}
</script>

<style scoped>
.v-list-item__content {
  /*flex: none;*/
  margin-right: 5px;
}
.v-input {
  flex: none;
  width: 80px;
}
</style>
