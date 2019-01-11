<template>
  <div class="game-listing" v-if="games.length > 0">
    <div class="row">
      <div class="col-12 mx-auto">
        <h2 class="list-title">{{title}}</h2>
      </div>
    </div>
    <div class="row">
      <div class="col-12 mx-auto">
        <GameItemList v-bind:games="games"/>
      </div>
    </div>
  </div>
</template>

<script>
import GameItemList from "./GameItemList.vue"

export default {
  name: "GameList",
  components: {
    GameItemList
  },
  props: {
    title: String,
    games: Array
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
