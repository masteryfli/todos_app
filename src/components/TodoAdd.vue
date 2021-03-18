<template>
  <div class="input-add">
      <input type="text" placeholder="请输入任务名称" v-model="name" @keyup.enter="add">
      <button @click="add">
        <i class="plus"></i>
      </button>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import { ITodo } from '../interface/todo'

export default defineComponent({
  name: 'TodoAdd',
  setup (props) {
    // 定义一个ref类型的数据
    const name = ref('')
    // 添加数据的方法
    const add = () => {
      // 获取文本框输入的数据
      const val = name.value
      // 如果没有输入数据，直接返回
      if (!val.trim()) return
      // 有数据，创建一个todo对象返回给app组件
      const todo: ITodo = {
        id: Date.now(),
        name: val,
        completed: false
      }
      // 调用 props 的 addTodo 方法
      props.addTodo(todo)
      // 清空输入框
      name.value = ''
    }
    return {
      add,
      name
    }
  },
  props: {
    addTodo: {
      type: Function,
      required: true
    }
  }
})
</script>

<style>
  /* 添加框 */
.input-add {
  position: relative;
  display: flex;
  align-items: center;
}
.input-add input {
  padding: 16px 52px 16px 18px;
  border-radius: 48px;
  border: none;
  outline: none;
  box-shadow: 0px 0px 24px rgba(0, 0, 0, 0.08);
  width: 100%;
  font-size: 16px;
  color: #626262;
}
.input-add button {
  width: 46px;
  height: 46px;
  border-radius: 50%;
  background: linear-gradient(#c0a5f3, #7f95f7);
  border: none;
  color: white;
  position: absolute;
  right: 0px;
  cursor: pointer;
  outline: none;
}
.input-add .plus {
  display: block;
  width: 100%;
  height: 100%;
  background: linear-gradient(#fff, #fff), linear-gradient(#fff, #fff);
  background-size: 50% 2px, 2px 50%;
  background-position: center;
  background-repeat: no-repeat;
}
</style>
