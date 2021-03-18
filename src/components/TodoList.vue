<template>
  <div class="todo-list">
    <TodoItem v-for="(todo, index) in todos"
    :key = "todo.id" :todo = "todo"
    :delTodo= "delTodo"
    :index= "index"
    :updateTodo = "updateTodo" />
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive } from 'vue'
import TodoItem from './TodoItem.vue'
import { ITodo } from '../interface/todo'

export default defineComponent({
  name: 'TodoList',
  setup (props) {
    let filterTodos = reactive<{filterTodos: ITodo[]}>({
      filterTodos: []
    })
    // console.log(props.getFilter())
    filterTodos = props.todos.filter((value: ITodo) => {
      if (props.status === 'done') {
        return value.completed === true
      } else if (props.status === 'todo') {
        return value.completed === false
      } else {
        return value
      }
    })
    return {
      filterTodos
    }
  },
  components: {
    TodoItem
  },
  props: ['todos', 'delTodo', 'updateTodo', 'status']
})
</script>

<style>
  .todo-list {
  display: grid;
  row-gap: 14px;
}
</style>
