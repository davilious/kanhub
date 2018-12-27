<template>
  <div id="app" class="container" v-show="!loading">
    <draggable :options="{group:'board', draggable: '.card'}" @start="drag=true" @end="onDragEnd" class="column column--todo" name="todo">
      <h1>Todo</h1>
      <Card v-for="(card, index) in cardsBy('todo')" :payload="card" :key="index" @card:drop="cardWasDrop"></Card>
    </draggable>
    <draggable :options="{group:'board', draggable: '.card'}" @start="drag=true" @end="onDragEnd" class="column column--development" name="development">
      <h1>Development</h1>
      <Card v-for="(card, index) in cardsBy('development')" :payload="card" :key="index" @card:drop="cardWasDrop"></Card>
    </draggable>
    <draggable :options="{group:'board', draggable: '.card'}" @start="drag=true" @end="onDragEnd"  class="column column--done" name="done">
      <h1>Done</h1>
      <Card v-for="(card, index) in cardsBy('done')" :payload="card" :key="index" @card:drop="cardWasDrop"></Card>
    </draggable>
  </div>
</template>

<script>
import draggable from 'vuedraggable'
import Card from './components/Card.vue';

export default {
  name: 'Kanhub',
  data() {
    return {
      currentCardId: null,
      cards: null,
      loading: true,
      drag: false
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
  components: { Card, draggable },
  methods: {
    cardsBy(state) {
      if (this.cards) {
        return this.cards.filter(card => card.state === state)
      }
    },
    cardWasDrop(id) {
      this.currentCardId = id
    },
    onDragEnd(evt) {
      const currentColumn = evt.to.getAttribute('name')

      axios
        .put(`http://localhost:8000/api/update/${this.currentCardId}`, {state: currentColumn})
        .then(response => {
          this.cards[response.data.id] = response.data
        })
    }
  }
}
</script>


