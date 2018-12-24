<template>
  <div id="app" class="container" v-show="!loading">
    <div class="column column--todo">
      <Card v-for="(card, index) in cardsBy('todo')" :payload="card" :key="index"></Card>
    </div>
    <div class="column column--development">
      <Card v-for="(card, index) in cardsBy('development')" :payload="card" :key="index"></Card>
    </div>
    <div class="column column--done">
      <Card v-for="(card, index) in cardsBy('done')" :payload="card" :key="index"></Card>
    </div>
  </div>
</template>

<script>
import Card from './components/Card.vue';

export default {
  name: 'Kanhub',
  data() {
    return {
      cards: null,
      loading: true
    }
  },
  mounted () {
    axios
      .get('http://localhost:8000/api/all')
      .then(response => {
        this.cards = response.data
      })
      .finally(() => this.loading = false)
  },
  components: { Card },
  methods: {
    cardsBy(state) {
      if (this.cards) {
        return this.cards.filter(card => card.state === state)
      }
    }
  }
}
</script>

<style scoped>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.container {
  display: flex;
  width: 100%;
}

.column {
  flex-basis: 33%;
  padding: 10px;
}

.column.column--todo {
  order: 1;
}

.column.column--development {
  order: 2;
}

.column.column--done {
  order: 3;
}
</style>

