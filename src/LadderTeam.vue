<template>
  <tr>
    <td class="name" :style="nameStyle">{{team.name}}</td>
    <td>{{played}}</td>
    <td>{{team.won}}</td>
    <td>{{team.drawn}}</td>
    <td>{{team.lost}}</td>
    <td>{{team.points}}</td>
    <td>{{team.percentage}}</td>

  </tr>
</template>

<script>
export default {
  name: 'LadderTeam',
  props: ['team', 'index', 'year'],
  computed: {
    key: function() {
      return this.team['.key'];
    },
    played: function(){
      return this.team.won + this.team.drawn + this.team.lost;
    },
    nameStyle: function(){
      return {
        backgroundImage: `url('/static/${this.key}.png')`,
        paddingLeft: '3.5em'
      };
    }
  }
}
</script>

<style>
  td{
    vertical-align: bottom;
  }
  .name {
    background-repeat: no-repeat;
    background-size: 3em 100%;

  }
</style>
