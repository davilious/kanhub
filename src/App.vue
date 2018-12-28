<template>
  <div id="app" class="container" v-show="!loading">
    <Addtodo
      :reset="this.showModal"
      v-show="this.showModal"
      @add-todo:add="addCardTodo"
      @add-todo:cancel="hideAddTodoModal">
    </Addtodo>
    <draggable :options="{group:'board', draggable: '.card'}" @start="drag=true" @end="onDragEnd" class="column column--todo" name="todo">
      <h1>Todo <a href="#" @click="showAddTodoModal"> + </a></h1>
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
import Addtodo from './components/Addtodo.vue';

export default {
  name: 'Kanhub',
  data() {
    return {
      currentCardId: null,
      cards: null,
      loading: true,
      drag: false,
      showModal: false
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
  components: { Card, Addtodo, draggable },
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
    },
    addCardTodo(payload) {
      axios
        .post('http://localhost:8000/api/add', payload)
        .then(response => {
          this.cards = this.cards.concat([response.data])
          this.showModal = false
        })
    },
    showAddTodoModal(e) {
      e.preventDefault()
      this.showModal = true
    },
    hideAddTodoModal() {
      this.showModal = false
    }
  }
}
</script>


