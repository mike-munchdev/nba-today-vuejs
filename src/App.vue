<template>
  <div id="app">
    <div class="container" v-if="isLoading === true">
      <div class="col-12 mx-auto text-center loading-icon-small">
        <i class="fa fa-spinner fa-3x fa-spin"/>
      </div>
    </div>
    <div class="container" v-if="isLoading === false">
      <div class="row">
        <div class="col-xs-12 col-md-8 offset-md-2 col-lg-6 offset-lg-3">
          <button class="btn btn-outline-primary btn-block mt-2" v-on:click="getGames">Refresh</button>
          <GameList title="Live" v-bind:games="liveGames"/>
          <GameList title="Upcoming" v-bind:games="upcomingGames"/>          
          <GameList title="Finished" v-bind:games="finishedGames"/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import GameList from "./components/GameList.vue";
import GameService from "./services/GameService.js";
import "font-awesome/css/font-awesome.min.css";
import "bootstrap/dist/css/bootstrap.css";

export default {
  name: "app",
  components: {
    GameList
  },
  data() {
    return {
      upcomingGames: [],
      liveGames: [],
      finishedGames: [],
      isLoading: false
    };
  },
  methods: {
    async getGames() {
      try {
        this.isLoading = true;
        const games = await GameService.getGames(new Date());
        this.upcomingGames = games.filter(g => g.statusNum === 1);
        this.liveGames = games.filter(g => g.statusNum === 2);
        this.finishedGames = games.filter(g => g.statusNum === 3);
        this.isLoading = false;
      } catch (e) {
        this.isLoading = false;
        alert("error occured");
        console.log(e);
      }
    }
  },
  created() {
    this.getGames();
  }
};
</script>

<style>
body {
  padding-bottom: 2.5em;
}
#loading {
  margin-top: 2.5rem;
}
#alert {
  margin-top: 2.5rem;
}
.hidden {
  display: none;
}
.team-record {
  font-size: 0.85rem;
  font-weight: 700;
}

.team-score {
  font-size: 1.25rem;
  font-weight: 700;
}
.game-time {
  font-size: 0.8rem;
}
.team-row {
  margin-bottom: 0.5rem !important;
}
.team-name {
  font-size: 1rem;
  font-weight: 700;
}
.game {
  margin-bottom: 1em;
}
.list-title {
  font-size: 1rem;
}
.game-listing {
  margin-top: 2.5rem;
}
.loading-icon-small {
  margin-top: 2.5rem;
}
</style>
