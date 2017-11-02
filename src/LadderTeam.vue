<template>
  <tr @click="clicked" :class="{ selected: isSelected }">
    <td><team-logo :code="code" :name="team.name"/></td>
    <td>{{played}}</td>
    <td>{{team.won}}</td>
    <td>{{team.drawn}}</td>
    <td>{{team.lost}}</td>
    <td>{{team.points}}</td>
    <td>{{team.percentage}}</td>
  </tr>
</template>

<script>
import TeamLogo from './TeamLogo.vue';

export default {
  name: 'LadderTeam',
  props: ['team', 'index', 'year', 'selectedTeam'],
  computed: {
    code: function() {
      return this.team['.key'];
    },
    played: function(){
      return this.team.won + this.team.drawn + this.team.lost;
    },
    isSelected: function(){
      return this.code == this.selectedTeam;
    }
  },
  methods: {
    clicked: function(){
      this.$emit('selected', this.code);
    }
  },
  components: {
    TeamLogo
  }
}
</script>

<style>
  tr{
    cursor: pointer;
  }
  td{
    vertical-align: bottom;
  }
  .selected{
    background-color: #ddf !important;
  }
</style>
