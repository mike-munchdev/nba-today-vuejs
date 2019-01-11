<template>
  <div>
    <ul class="list-group">
      <GameItem v-bind:game="game" v-bind:key="game.gameId" v-for="game in games"/>
    </ul>
  </div>
</template>

<script>
import GameItem from "./GameItem.vue";
export default {
  name: "GameItemList",
  components: {
    GameItem
  },
  props: ["games"]
};
</script>

<style scoped>
</style>
