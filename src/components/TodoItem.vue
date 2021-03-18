<template>
  <div class="todo-item"
    @mouseenter="mouseHander(true)"
    @mouseleave="mouseHander(false)"
    :class="{ done:isCompeted }">
    <label>
      <input type="checkbox" v-model = "isCompeted" :checked= "isCompeted">
      {{ todo.name }}
      <span class="check-button"></span>
      <button v-show="showDel" @click="deleteTodo">
        <i class="delete-button"></i>
      </button>
    </label>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent, ref } from 'vue'
import { ITodo } from '../interface/todo'

export default defineComponent({
  name: 'TodoItem',
  setup (props) {
    const showDel = ref(false)
    const mouseHander = (flag: boolean) => {
      if (flag === true) {
        showDel.value = true
      } else {
        showDel.value = false
      }
    }
    const deleteTodo = () => {
      if (window.confirm('确定要删除该记录吗？')) {
        props.delTodo(props.index)
      }
    }
    // 通过计算属性确定复选框选中/补选中
    const isCompeted = computed({
      get () {
        return props.todo.completed
      },
      set (state) {
        props.updateTodo(props.todo, state)
      }
    })
    return {
      showDel,
      mouseHander,
      deleteTodo,
      isCompeted
    }
  },
  props: {
    todo: {
      // Object 转换成 todo 类型
      type: Object as () => ITodo,
      required: true
    },
    delTodo: {
      type: Function,
      required: true
    },
    index: {
      type: Number,
      required: true
    },
    updateTodo: {
      type: Function,
      required: true
    }
  }
})
</script>

<style>
  .todo-item {
    background: white;
    padding: 16px;
    border-radius: 8px;
    color: #626262;
  }
  .todo-item:hover {
    -webkit-transform: translateY(-3px);
    -ms-transform: translateY(-3px);
    transform: translateY(-3px);
    -webkit-box-shadow: 0 0 6px #ccc;
    box-shadow: 0 0 6px #ccc;
    -webkit-transition: all .5s ease-out;
    transition: all .2s ease-out;
  }
  .todo-item label {
    position: relative;
    display: flex;
    align-items: center;
    cursor: pointer;
  }
  .todo-item.done label {
    text-decoration: line-through;
    font-style: italic;
  }
  .todo-item label span.check-button {
    position: absolute;
    top: 0;
  }
  .todo-item label span.check-button::before,
  .todo-item label span.check-button::after {
    content: "";
    display: block;
    position: absolute;
    width: 18px;
    height: 18px;
    border-radius: 50%;
  }
  .todo-item label span.check-button::before {
    border: 1px solid #b382f9;
  }
  .todo-item label span.check-button::after {
    transition: 0.4s;
    background: #b382f9;
    transform: translate(1px, 1px) scale(0.8);
    opacity: 0;
  }
  .todo-item input {
    margin-right: 16px;
    opacity: 0;
  }
  .todo-item input:checked + span.check-button::after {
    opacity: 1;
  }
  .todo-item label button {
    width: 24px;
    height: 24px;
    border-radius: 50%;
    background: linear-gradient(#dbc6c7, #d94a4a);
    border: none;
    color: white;
    position: absolute;
    right: 0px;
    cursor: pointer;
    outline: none;
  }
  .todo-item label button i {
    display: block;
    width: 100%;
    height: 100%;
    background: linear-gradient(#fff, #fff), linear-gradient(#fff, #fff);
    background-size: 80% 1px, 0, 0;
    background-position: center;
    background-repeat: no-repeat;
  }
</style>
