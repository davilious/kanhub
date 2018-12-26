<template>
  <div v-bind:class="generateClassByState(payload.state)" v-on:drop="onDrop(payload._id)">
    <span>{{ payload.title }}</span>
    <span>{{ payload.state }}</span>
  </div>
</template>

<script>
  export default {
    name: 'Card',

    props: {
      payload: Object
    },

    methods: {
      generateClassByState(state) {
        return `card card--${state}`
      },
      onDrop(id) {
        this.$emit('card:drop', id)
      }
    }
  }
</script>

<style>
  .card {
    display: flex;
    flex-direction: column;
    height: 150px;
    border: 1px solid orange;
    margin-bottom: 10px;
  }
</style>
