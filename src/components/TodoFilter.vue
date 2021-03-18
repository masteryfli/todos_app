<template>
  <div class="filters">
    <input type="checkbox" v-model="checkedAll" />
    <span class="check-button"></span>
    <!-- <span v-for="filter in filters" :key="filter.value" class="filter"
    @click="filterHander(filter.value)">
    {{ filter.label }} <i>{{getCount()}}</i>
    </span> -->
    <span class="filter">全部<i class="filter-count">{{ todos.length }}</i></span>
    <span class="filter">未完成<i class="filter-count todo">{{ unComputedCount }}</i></span>
    <span class="filter">已完成<i class="filter-count done">{{ computedCount }}</i></span>
    <span class="iconfont filter-clear" @click="clearComputed">&#xe689;</span>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent } from 'vue'
import { ITodo } from '../interface/todo'

export default defineComponent({
  name: 'TodoFilter',
  setup (props) {
    // const filters = [
    //   { label: '全部', value: 'all' },
    //   { label: '已完成', value: 'done' },
    //   { label: '未完成', value: 'todo' }
    // ]
    const filterHander = (tag: string) => {
      props.getFilter(tag)
    }
    const computedCount = computed(() => {
      return props.todos.reduce((pre, todo) => pre + (todo.completed ? 1 : 0), 0)
    })
    const unComputedCount = computed(() => {
      return props.todos.reduce((pre, todo) => pre + (!todo.completed ? 1 : 0), 0)
    })
    const checkedAll = computed({
      get () {
        return computedCount.value > 0 && props.todos.length === computedCount.value
      },
      set (val) {
        props.checkAll(val)
      }
    })
    // const clearComputedHander = () => {
    //   if (window.confirm('是否清空已完成成任务？')) {
    //     props.clearComputed()
    //   }
    // }
    return {
      // filters,
      filterHander,
      checkedAll,
      computedCount,
      unComputedCount
    //  clearComputedHander
    }
  },
  props: {
    getFilter: {
      type: Function,
      required: true
    },
    todos: {
      type: Array as () => ITodo [],
      required: true
    },
    checkAll: {
      type: Function,
      required: true
    },
    clearComputed: {
      type: Function,
      required: true
    }
  }
})
</script>

<style>
  .filters {
    display: flex;
    margin: 24px 2px;
    color: #c0c2ce;
    font-size: 14px;
    cursor: pointer;
    position: relative;
  }
  .filters .filter {
    margin-right: 14px;
    transition: 0.8s;
  }
  .filters .filter.active {
    color: #6b729c;
    transform: scale(1.2);
  }
  .filters span.check-button {
    position: absolute;
    top: -1px;
  }
  .filters span.check-button::before,
  .filters span.check-button::after {
    content: "";
    display: block;
    position: absolute;
    width: 18px;
    height: 18px;
    border-radius: 50%;
  }
  .filters span.check-button::before {
    border: 1px solid #b382f9;
  }
  .filters span.check-button::after {
    transition: 0.4s;
    background: #b382f9;
    transform: translate(1px, 1px) scale(0.8);
    opacity: 0;
  }
  .filters input {
    margin-right: 16px;
    opacity: 0;
    z-index: 1;
    cursor: pointer;
  }
  .filters input:checked + span.check-button::after {
    opacity: 1;
  }
  .filters span i.filter-count {
    font-weight: bold;
    color: #000;
  }
  .filters span i.filter-count.todo {
    color: orange;
  }
  .filters span i.filter-count.done  {
    color: green;
  }
  .filters span.filter-clear {
    position: absolute;
    right: 10px;
  }
</style>
