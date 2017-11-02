<template>
  <div>
    <h1>
      <team-logo :code="team" :name="name">
        <span slot="suffix">in {{year}}</span>
      </team-logo>
    </h1>
    <table class="table">
      <tbody>
        <match v-for="game in fixture" :db="db" :year="year" :game="game"
          :selectedTeam="team" :teams="teams" :venues="venues" :key="game['.value']"/>
      </tbody>
    </table>
  </div>
</template>

<script>
import TeamLogo from './TeamLogo.vue';
import Match from './Match.vue';
export default {
  name: 'Team',
  props: ['db', 'team', 'year', 'teams', 'venues'],
  data: function(){
    return {
      fixture: null,
      name: null
    };
  },
  methods: {
    update: function(){
      if(!this.team || !this.year){
          return;
      }
      this.$bindAsArray('fixture', this.db.ref('results').child(this.team).child(this.year));
      this.name = this.teams[this.team].name;
    }
  },
  watch: {
    team: function(val){ this.update() },
    year: function(val){ this.update() },
  },
  mounted: function(){
    this.update();
  },
  components: {
    TeamLogo,
    Match
  }
}
</script>
