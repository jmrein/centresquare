<template>
  <span class="logo" :style="style">
    <slot name="prefix"/>
    {{name}}
    <slot name="suffix"/>
  </span>
</template>

<script>
export default {
  name: 'LadderTeam',
  props: [ 'code', 'name'],
  computed: {
    style: function(){
      return { backgroundImage: `url('/static/${this.code}.png')`, };
    }
  }
}
</script>

<style>
.logo{
  background-repeat: no-repeat;
  background-size: 3em 100%;
  padding-left: 3.5em;
  width: 100%;
  height: 100%;
}
</style>
