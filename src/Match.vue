<template>
  <tr :style="resultStyle">
    <td class="round">{{game.name}}</td>
    <td>
      <team-logo :code="oppCode" :name="oppName" position="right">
        <span slot="prefix">{{result}}
          <span :class="{ isUs: isHome }">{{this.game.home.score}}</span>
          -
          <span :class="{ isUs: !isHome }">{{this.game.away.score}}</span>
          {{location}}</span>
      </team-logo>
    </td>
    <td>{{venue}}</td>
  </tr>
</template>

<script>
import TeamLogo from './TeamLogo.vue';
export default {
  name: 'Match',
  props: ['db', 'year', 'game', 'selectedTeam', 'teams', 'venues'],
  data: function(){
    return {
      oppCode: null,
      oppName: null,
      venue: null,
      location: null,
      result: null,
      isHome: true,
      resultStyle: {}
    };
  },
  methods: {
    calculateResult: function(score, against){
      if(score > against){
        this.result = 'Won';
        this.resultStyle.backgroundColor = '#cfc';
      }
      else if(score < against){
        this.result = 'Lost';
        this.resultStyle.backgroundColor = '#fcc';
      }
      else{
        this.result = 'Drawn';
        this.resultStyle.backgroundColor = '#eee';
      }
    }
  },
  mounted: function(){
    var opp  = null;
    if(this.game.home.team == this.selectedTeam){
      this.oppCode = this.game.away.team;
      this.location = 'vs';
      this.isHome = true;
      this.calculateResult(this.game.home.score, this.game.away.score)
    }
    else{
      this.oppCode = this.game.home.team;
      this.location = 'at';
      this.isHome = false;
      this.calculateResult(this.game.away.score, this.game.home.score)
    }
    this.oppName = this.teams[this.oppCode].name;
    this.venue = this.venues['v' + this.game.venue].name;
  },
  components: {
    TeamLogo
  }
}
</script>

<style>
.round {
  white-space: nowrap;
}
.isUs {
  font-weight: bold;
}
</style>
