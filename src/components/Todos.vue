<template>
  <div>
    <div v-for="(todo, index) in filterTodo" :key="todo.title" v-if="visibility === 'all'">
      <b-field class="is-pulled-left">
        <b-checkbox size="is-large" v-model="todo.complete" @input="statusTodo(index)">
          <strike v-if="todo.completed">{{ todo.title }}</strike>
          <span v-else >{{ todo.title }}</span>
        </b-checkbox>
      </b-field>
      <a class="delete is-pulled-right" @click="delTodo(index)" ></a>
      <div class="is-clearfix"></div>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
import Sortable from 'sortable'

export default {
  computed: {
    ...mapGetters([
      'todos',
      'visibility'
    ]),
    filterTodo: function () {
      if (this.visibility === 'active') {
        return this.todos.filter(todo => todo.completed === false)
      } else if (this.visibility === 'completed') {
        return this.todos.filter(todo => todo.completed === true)
      } else if (this.visibility === 'ClearCompleted') {
        for (let i = this.todos.length - 1; i >= 0; i--) {
          if (this.todos[i].completed) {
            this.todos.splice(i, 1)
          }
        }
      } else return this.todos
    }
  },
  mounted () {
    let table = document.getElementById('sorting')
    Sortable.create(table, { handle: '.handle', onUpdate: this.onUpdate, animation: 100})
  },
  methods: {
    ...mapActions([
      'delTodo',
      'statusTodo',
      'visibility',
      'sortTable'  
    ]),
    onUpdate: function (event) {
      this.sortTable({ newIndex: event.newIndex, oldIndex: event.oldIndex})
    }
  }
}
</script>