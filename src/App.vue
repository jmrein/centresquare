<template>
  <div id="app" class="container">
    <select v-model="year">
      <option v-for="y in years">{{y}}</option>
    </select>
    <div class="content">
      <ladder :data="ladder" :year="year" @selected="selected"/>
      <team :db="database" :year="year" :team="team" :teams="teams" :venues="venues"
       v-if="team" />
    </div>
  </div>
</template>

<script>
import Ladder from './Ladder.vue';
import Team from './Team.vue';
import Firebase from 'firebase';
var config = require('./config.json');
var app = Firebase.initializeApp(config.firebase);
var db = app.database();
var ladders = db.ref('ladders');

export default {
  name: 'app',
  firebase: function() {
    return {
      range: { source: db.ref('range'), asObject: true },
      ladder: ladders.child(this.year),
      teams: { source: db.ref('teams'), asObject: true },
      venues: { source: db.ref('venues'), asObject: true }
     };
  },
  data: function() {
    return {
      year: new Date().getFullYear(),
      team: null
    };
  },
  methods: {
    selected: function(teamCode){
      this.team = teamCode;
    }
  },
  computed: {
    years: function(){
      if (!this.range.start) {
        return new Array();
      }
      return new Array(this.range.end + 1 - this.range.start).fill()
        .map((d, i) => i + this.range.start).reverse();
    },
    database: function(){
      return db;
    }
  },
  watch: {
    year: function(val){
      this.$bindAsArray('ladder', ladders.child(val));
    }
  },
  components: {
    Ladder,
    Team
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
