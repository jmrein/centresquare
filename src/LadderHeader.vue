<template>
  <th @click="sort">{{title}}</th>
</template>

<script>
export default {
  name: 'LadderHeader',
  props: ['column', 'title', 'defaultOrder'],
  methods: {
    sort: function(){
      this.$emit('sort', this.column, this.defaultOrder);
    }
  }
}
</script>

<style scoped>
  th{
    cursor: pointer;
  }
</style>
