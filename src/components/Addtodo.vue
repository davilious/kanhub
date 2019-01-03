<template>
  <div class="addtodo">
    <div class="addtodo-modal">
      <form>
      	<h1>Add todo</h1>
        <div class="row u-push-bottom">
          <input type="text" v-model="title" placeholder="add title">
      	</div>
      	<div class="row u-push-bottom">
          <input type="text" v-model="app_name" placeholder="add app: e.g. id, sequence, movida…">
      	</div>
      	<div class="row u-push-bottom">
          <input type="text" v-model="tags" placeholder="add tags: e.g. needs design, urgent…">
      	</div>
      	<div class="row u-push-bottom">
          <input type="text" v-model="users" placeholder="add users: e.g. davilious, demimismo">
      	</div>
      	<button v-on:click="addCardTodo">Add todo</button> or <a href="#" class="u-font-small" v-on:click="cancelAddtodo">cancel</a>
      </form>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Addtodo',
    props: {
      reset: {
        type: Boolean,
        default: false
      }
    },
    data() {
      return {
        title: null,
        app_name: null,
        tags: null,
        users: null
      }
    },
    methods: {
      cancelAddtodo() {
        this.resetValues()
        this.$emit('add-todo:cancel')
      },
      addCardTodo() {
        this.$emit('add-todo:add', {
          title: this.title,
          app_name: this.app_name,
          tags: this.tags,
          users: this.users
        })
      },
      resetValues() {
        this.title = null;
        this.app_name = null;
        this.tags = null;
        this.users = null;
      }
    },
    watch: {
      reset() {
        this.resetValues()
      }
    }
}
</script>

<style scoped>
  .addtodo {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    min-height: 100vh;
    background: rgba(0,0,0, .2);
    z-index: 9000;
  }

  .addtodo-modal {
    position: relative;
  	padding: 20px;
    width: 400px;
    background: white;
    border: 1px solid #d4d7e1;
    margin-left: 135px;
    margin-top: 15px;
    z-index: 1;
  }

  .addtodo-modal:before {
    position: absolute;
    content: '';
    display: block;
    width: 20px;
    height: 20px;
    background: red;
    background: white;
    border: 1px solid #d4d7e1;
    z-index: 10;
    transform: rotate(-45deg);
    border-right: none;
    border-bottom: none;
    left: -11px;
  }

  .addtodo-modal h1 {
    font-size: 1em;
    margin: 0 0 20px;
  }
</style>
