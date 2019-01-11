<template>
  <li class="list-group-item game">
    <div v-html="gameStart"></div>
    <div v-html="vTeamLine"></div>
    <div v-html="hTeamLine"></div>
  </li>
</template>

<script>
export default {
  name: "GameItem",
  props: ["game"],
  methods: {
    getPeriodWithOrdinal(period) {
      switch (period) {
        case 1:
          return "1st";
        case 2:
          return "2nd";
        case 3:
          return "3rd";
        case 4:
          return "4th";
        default:
          return "OT";
      }
    },
    getScore(game, team) {
      if (game.period.current === 0) {
        return `<span class="team-record">${team.win} - ${team.loss}</span>`;
      } else {
        return `<span class="team-score">${team.score}</span>`;
      }
    },
    getTeamLogoUrl(triCode) {
      return `assets/${triCode}.png`;
    },

    getTeamLine(game, team) {
      return `<div class="row team-row">
        <div class="col-2">
          <img
            src=${this.getTeamLogoUrl(team.triCode)}
            alt='${team.fullName} logo'
            width="30"
            height="30"
            class="float-right"
          />
        </div>
        <div class="col-7">
          <span class="team-name text-center h-100">${team.fullName}</span>
        </div>
        <div class="col-3 text-right">${this.getScore(game, team)}</div>
      </div>`;
    },
    getGameStart(game) {
      if (game.period.isHalftime === true) {
        return `<div class="game-time text-right font-weight-bold">Half</div>`;
      } else if (game.period.current === 0) {
        const startTime = new Date(game.startTimeUTC);
        return `<div class="game-time text-right text-muted">
            ${startTime.toLocaleTimeString([], {
              hour: "2-digit",
              minute: "2-digit",
              timeZoneName: "short"
            })}
          </div>`;
      } else {
        switch (game.statusNum) {
          case 2:
            return `<div class="game-time text-right">
                ${game.period.isEndOfPeriod ? "End of " : game.clock}
                ${this.getPeriodWithOrdinal(game.period.current)}
              </div>`;
          case 3:
            return `<div class="game-time text-right text-muted font-weight-bold">
                Final
              </div>`;
          default:
            return `<div class="game-time text-right text-muted" />`;
        }
      }
    }
  },
  computed: {
    gameStart: function() {
      return this.getGameStart(this.game);
    },
    vTeamLine: function() {
      return this.getTeamLine(this.game, this.game.vTeam);
    },
    hTeamLine: function() {
      return this.getTeamLine(this.game, this.game.hTeam);
    }
  }
};
</script>

<style scoped>
</style>


