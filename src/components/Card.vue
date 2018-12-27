<template>
  <div v-bind:class="generateClassByState(payload.state)" v-on:drop="onDrop(payload._id)">
    <div class="card-heading">
      <div class="card-heading__logo" v-bind:class="addClassForLogo(payload.app_name)">
      </div>
      <div class="card-heading__text">
        {{ payload.app_name }}
      </div>
    </div>
    <div class="card-body">
      <span class="card-body__title">{{ payload.title }}</span>
    </div>
    <div class="card-foot">
      <div class="card-foot__tags" v-html="generateTags(payload.tags)"></div>
      <div class="card-foot__users" v-html="generateAvatars(payload.users)"></div>
    </div>
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
      },
      addClassForLogo(app_name) {
        return `card-heading__logo--${app_name}`
      },
      generateAvatars(users) {
        return users.map( user => `<img src="https://github.com/${user}.png?size=30"> `).join('')
      },
      generateTags(tags) {
        return tags.map(tag => `<span class="card-foot__tags-tag card-foot__tags-tag--${tag}">${tag}</span>`).join('')
      }
    }
  }
</script>

<style>
  .card {
    position: relative;
    background: #FFFFFF;
    display: flex;
    flex-direction: column;
    height: 150px;
    margin-bottom: 10px;
    color: #626072;
    padding: 20px;
    text-align: left;
    border: 1px solid #d4d7e1;
  }

  .card:hover {
    cursor: move;
  }

  .card-heading {
    display: flex;
  }

  .card-heading__logo {
    width: 50px;
    height: 50px;
    background-repeat: no-repeat;
    background-size: 50%;
  }

  .card-heading__logo--movida {
    background-image: url('/images/logo-movida.svg');
  }

  .card-heading__logo--sequence {
    background-image: url('/images/logo-sequence.svg');
  }

  .card-heading__logo--id {
    background-image: url('/images/logo-id.svg');
  }

  .card-heading__text {
    font-weight: 700;
    font-size: 1em;
    line-height: 40px;
  }

  .card-body {
    margin-left: 50px;
    font-size: 0.9em;
  }

  .card-foot {
    display: flex;
    margin-left: 45px;
    justify-content: space-between;
  }

  .card-foot {
    display: flex;
    justify-content: space-between;
    width: calc(100% - 95px);
    position: absolute;
    bottom: 20px;
  }

  .card-foot__tags {
    flex-basis: 50%
  }

  .card-foot__tags-tag {
    position: relative;
    padding: 5px 8px;
    background: #97A0AE;
    color: white;
    font-size: 0.7em;
    margin-right: 2px;
    border-radius: 3px;
  }

  .card-foot__tags-tag--urgent {
    background: #9B374E;
  }

  .card-foot__tags-tag:before {
    content: '•';
    vertical-align: middle;
    margin-right: 5px;
    color: rgba(0,0,0,.4);
  }

  .card-foot__users {
    display: flex;
    justify-content: center;
  }

  .card-foot__users > img {
    position: absolute;
    border-radius: 100%;
    border: 2px solid white;
  }

  .card-foot__users > img:not(:first-child) {
    margin-left: -20px;
  }

  .card-foot__users > img:nth-child(3) {
    margin-left: -40px;
  }

  .card:hover {
    box-shadow: 0 0 10px rgba(0,0,0,0.05), 0 0 20px rgba(0,0,0,0.05);;
  }

  .sortable-chosen {
    cursor: move;
    box-shadow: 0 0 10px rgba(0,0,0,0.05), 0 0 20px rgba(0,0,0,0.05);
  }

</style>
