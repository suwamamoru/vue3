<template>
<div id="todo">
  <h1>ToDoリスト</h1>
  <input type="radio" v-model="filter" name="group" value="all">すべて
  <input type="radio" v-model="filter" name="group" value="working">作業中
  <input type="radio" v-model="filter" name="group" value="finish">完了

  <table>
    <tr>
      <th>ID</th>
      <th>コメント</th>
      <th>状態</th>
    </tr>
    <tbody v-for="(todo,index) in filteredTodos" :key="index">
      <th>{{todo.i}}</th>
      <th>{{todo.item}}</th>
      <th><button @click="taskChange(todo.i)">{{todo.state}}</button></th>
      <th><button @click="deleteItem(todo.i)">削除</button></th>
    </tbody>
  </table>

  <h2>新規タスクの追加</h2>
  <form @submit.prevent>
    <input type="text" v-model="newItem">
    <button @click="addItem">追加</button>
  </form>
</div>
</template>

<script>
export default {
  data() {
    return {
      newItem: '',
      todos: [],
      filter: 'all'
    }
  },
  computed: {
    filteredTodos() {
      if(this.filter == 'all') {
        return this.todos
      } else if(this.filter == 'working') {
        return this.todos.filter(todo => todo.state == '作業中')
      } else {
        return this.todos.filter(todo => todo.state == '完了')
      }
    }
  },
  methods: {
    addItem() {
      if(this.newItem == '')return
      const todo = {
        item: this.newItem,
        state: '作業中',
        i: Object.keys(this.todos).length
      }
      this.todos.push(todo)
      this.newItem = ''
    },
    taskChange(i) {
      if(this.todos[i].state == '作業中') {
        this.todos[i].state = '完了'
      } else {
        this.todos[i].state = '作業中'
      }
    },
    deleteItem(i) {
      this.todos.splice(i,1)
      for (let k = 0; k < Object.keys(this.todos).length; k++) {
        Object.values(this.todos)[k].i = Object.keys(this.todos)[k]
      }
    }
  }
}
</script>
