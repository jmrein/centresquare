<template>
  <table class="table table-striped">
    <thead>
      <tr>
        <ladder-header v-for="column in columns" :column="column.column"
          :title="column.title" :defaultOrder="column.defaultOrder"
          @sort="sortBy" :key="column.column"/>
      </tr>
    </thead>
    <tbody>
      <ladder-team v-for="(team, index) in teams" :team="team" :index="index"
        :year="year" :key="team.key"/>
    </tbody>
  </table>
</template>

<script>
import LadderHeader from './LadderHeader.vue';
import LadderTeam from './LadderTeam.vue';
import 'bootstrap/dist/css/bootstrap.css';
import 'bootstrap-vue/dist/bootstrap-vue.css';
import 'lodash';
export default {
  name: 'Ladder',
  props: ['data', 'year'],
  components: {
    LadderHeader,
    LadderTeam,
  },
  data: function(){
    return {
      sortKey: ['points', 'percentage'],
      sortOrder: ['desc', 'desc'],
      reverse: false,
      columns: [
        { column: "name", title: "Club", defaultOrder: "asc"},
        { column: "played", title: "Played", defaultOrder: "desc"},
        { column: "won", title: "Won", defaultOrder: "desc"},
        { column: "drawn", title: "Drawn", defaultOrder: "desc"},
        { column: "lost", title: "Lost", defaultOrder: "desc"},
        { column: "points", title: "Points", defaultOrder: "desc"},
        { column: "percentage", title: "%", defaultOrder: "desc"}
      ]
    }
  },
  computed: {
    teams: function () {
      var teams = _.orderBy(this.data, this.sortKey, this.sortOrder);
      for (var i = 0, len = teams.length; i < len; i++) {
        teams[i].index = i;
      }
      return teams;
    }
  },
  methods: {
    sortBy: function(sortKey, sortOrder) {
      if(this.sortKey[0] == sortKey && this.sortOrder[0] == 'asc'){
        this.sortOrder = ['desc', 'asc'];
      }
      else if (this.sortKey[0] == sortKey && this.sortOrder[0] == 'desc'){
          this.sortOrder = ['asc', 'asc'];
        }
      else {
        this.sortKey =  [sortKey, 'index'];
        this.sortOrder = [sortOrder, 'asc'];
      }
    }
  }
}
</script>

<style scoped>
  th{
    cursor: pointer;
  }
</style>
