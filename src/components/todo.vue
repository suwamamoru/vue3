<template>
<div id="todo">
  <h1>ToDoリスト</h1>
  <input type="radio" name="group" value="all" checked  @change="isShow($event)">すべて
  <input type="radio" name="group" value="working" @change="isShow($event)">作業中
  <input type="radio" name="group" value="finish" @change="isShow($event)">完了

  <table>
    <tr>
      <th>ID</th>
      <th>コメント</th>
      <th>状態</th>
    </tr>
    <tbody v-for="(todo,index) in todos" :key="index" :class="{work: todo.isWork, done: todo.isDone}" v-show="isShow">
      <th>{{todo.i}}</th>
      <th>{{todo.item}}</th>
      <th><button @click="taskChange(index)">{{todo.condition}}</button></th>
      <th><button @click="deleteItem(index)">削除</button></th>
    </tbody>
  </table>

  <h2>新規タスクの追加</h2>
  <form @submit.prevent>
    <input type="text" v-model="newItem">
    <button @click="addItem">追加</button>
  </form>

  <pre>{{ $data }}</pre>

</div>
</template>

<script>
export default {
  data() {
    return {
      newItem: '',
      todos: [],
      val: 'all'
    }
  },
  methods: {
    addItem() {
      if(this.newItem == '')return
      const todo = {
        item: this.newItem,
        isWork: true,
        isDone: false,
        condition: '作業中',
        i: Object.keys(this.todos).length
      }
      this.todos.push(todo)
      this.newItem = ''
    },
    taskChange(index) {
      if(this.todos[index].isDone == false) {
        this.todos[index].isWork = false
        this.todos[index].isDone = true
        this.todos[index].condition = '完了'
      } else {
        this.todos[index].isWork = true
        this.todos[index].isDone = false
        this.todos[index].condition = '作業中'
      }
      this.dispChange()
    },
    deleteItem(index) {
      this.todos.splice(index,1)
      for (let k = 0; k < Object.keys(this.todos).length; k++) {
        Object.values(this.todos)[k].i = Object.keys(this.todos)[k]
      }
    },
    isShow(event) {
      this.val = event.target.value
      this.dispChange()
    },
    dispChange() {
      const workTask = document.querySelectorAll('.work')
      const finishTask = document.querySelectorAll('.done')
      console.log(workTask)
      console.log(finishTask)
      if(this.val == 'all') {
        workTask.forEach(element => {
          element.style.display = ''
        })
        finishTask.forEach(element => {
          element.style.display = ''
        })
      } else if (this.val == 'working') {
        workTask.forEach(element => {
          element.style.display = ''
        })
        finishTask.forEach(element => {
          element.style.display = 'none'
        })
      } else {
        workTask.forEach(element => {
          element.style.display = 'none'
        })
        finishTask.forEach(element => {
          element.style.display = ''
        })
      }
    }
  }
}
</script>
