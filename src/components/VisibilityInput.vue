<template>
  <div>
    <b-field class="is-pulled-right">
      <b-radio-button v-model="visibility"
        native-value="all">
        <span>All ( {{all}})</span>
      </b-radio-button>
      <b-radio-button v-model="visibility"
        native-value="active">
        <span>Active ( {{active}} )</span>
      </b-radio-button>
      <b-radio-button v-model="visibility"
        native-value="completed">
        <span>Completed ( {{Completed}} )</span>
      </b-radio-button>
      <b-radio-button v-model="visibility"
        native-value="ClearCompleted" >
        <span>Clear Completed</span>
      </b-radio-button>
      <button class="button is-link" @click="clearTodos()">
        <b-icon
          icon="delete"
          size="is-small"
          type="is-white">
        </b-icon>
        <span>Clear Todos Completed</span>
      </button>
    </b-field>
  </div>
</template>

<script>
import { mapActions, mapGetters } from 'vuex'
import { store } from '@/store'
export default {
  computed: {
    ...mapGetters([
      'todos',
      'visibility'
    ]),
    all () {
      if (this.todos) {
        return this.todos.length
      }
    },
    active () {
      if (this.todos) {
        return this.todos.filter(todo => todo.completed === false).length
      }
    },
    Completed () {
      if (this.todos) {
        return this.todos.filter(todo => todo.completed === true).length
      }
    },
    visibility: {
      get: function () {
        return store.state.visibility
      },
      set: function (newValue) {
        store.dispatch('changeVisibility', newValue)
      }
    },
    countAll: function () {
      var result = 0
      for (var i = 0; i < this.todos.length; i++) {
        result++
      }
      return result
    },
    countActive: function () {
      var result = 0
      for (var i = 0; i < this.todos.length; i++) {
        if (!this.todos[i].completed) result++
      }
      return result
    },
    countCompleted: function () {
      var result = 0
      for (var i = 0; i < this.todos.length; i++) {
        if (this.todos[i].completed) result++
      }
      return result
    }
  },
  methods: {
    ...mapActions([
      'clearTodos'
    ])
  }
}
</script>
