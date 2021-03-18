<template>
  <main>
    <div class="container">
      <h1>欢迎使用 YFL 待办事项</h1>
      <TodoAdd :addTodo = "addTodo"/>
      <TodoFilter :getFilter = "getFilter" :todos= "todos" :checkAll="checkAll" :clearComputed="clearComputed"/>
      <TodoList :todos = "todos" :delTodo= "delTodo" :updateTodo = "updateTodo" :status="status"/>
    </div>
  </main>
</template>

<script lang="ts">
import { defineComponent, onMounted, reactive, ref, toRefs, watch } from 'vue'
import TodoAdd from './components/TodoAdd.vue'
import TodoList from './components/TodoList.vue'
import TodoFilter from './components/TodoFilter.vue'
import { ITodo } from './interface/todo'
import { saveTodos, readTodos } from './utils/storage'
export default defineComponent({
  name: 'App',
  setup () {
    const state = reactive<{todos: ITodo[] }>({
      // todos: [
      //   { id: 1, name: 'Java', completed: true },
      //   { id: 2, name: 'c#', completed: true },
      //   { id: 3, name: 'JavaScript', completed: false }
      // ]
      todos: []
    })
    const status = ref<string>('all')

    // 页面加载完毕之后从 localstorage 中读取数据
    onMounted(() => {
      setTimeout(() => {
        state.todos = readTodos()
      }, 100)
    })

    // 向数组中添加数据
    const addTodo = (todo: ITodo) => {
      state.todos.unshift(todo)
    }
    // 删除数组中的一条数据
    const delTodo = (index: number) => {
      state.todos.splice(index, 1)
    }
    // 修改 todo 数组中某一条数据的状态
    const updateTodo = (todo: ITodo, completed: boolean) => {
      todo.completed = completed
    }
    // 过滤全部/已完成/未完成的数据
    const getFilter = (tag = 'all') => {
      status.value = tag
    }
    // 全选/全不选
    const checkAll = (isComputed: boolean) => {
      // 遍历数组，更新todos 的所有状态
      state.todos.forEach((todo) => {
        todo.completed = isComputed
      })
    }
    // 清除所有完成的数据
    const clearComputed = () => {
      state.todos = state.todos.filter((todo) => { return !todo.completed === true })
    }

    // 用 watch 监听数据的变化
    // watch (() => state.todos, (value) => {
    //   localStorage.setItem("todos_key", JSON.stringify(value))
    // }, { deep: true })
    // watch (() => state.todos, (value) => {
    //   saveTodos(value)
    // }, { deep: true })
    watch(() => state.todos, saveTodos, { deep: true })
    return {
      // 解构出todos
      ...toRefs(state),
      addTodo,
      delTodo,
      updateTodo,
      getFilter,
      status,
      checkAll,
      clearComputed
    }
  },
  components: {
    TodoAdd,
    TodoList,
    TodoFilter
  }
})
</script>

<style>
  @import '~@/assets/icon/iconfont.css';
  /**整个页面 */
  main {
    width: 100vh;
    min-height: 100vh;
    padding: 10vh 0;
    display: grid;
    align-items: start;
    justify-items: center;
    background-color: #d8dfff;
  }
  .container {
    width: 60%;
    max-width: 400px;
    box-shadow: 0px 0px 24px rgba(0, 0, 0, .15);
    border-radius: 24px;
    padding: 48px 28px;
    background-color: rgb(245, 246, 252);
  }

  /**标题 */
  h1 {
    margin: 24px 0;
    font-size: 28px;
    color: #414873;
    text-align: center;
  }
</style>
