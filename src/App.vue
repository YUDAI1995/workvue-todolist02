<template>
<header>
  <div class="inner">
    <h1>todolist</h1>
  </div>
</header>
<main>
  <div class="inner">
    <InputForm @onsubmit='addTask_socketio'></InputForm>
    <p>{{$data.task}}</p>
  </div>
  <div class="inner">
    <TodoList :todoList="$data.todoList" @check="onCheckTodo" @delete="onDeleteTodo" />
  </div>
</main>
<footer>
  <div class="inner">
    <p><small class="copy">work</small></p>
  </div>
</footer>
</template>

<script>
import InputForm from './components/InputForm.vue'
import TodoList from './components/TodoList.vue'
import io from 'socket.io-client';
import _ from 'lodash'

export default {
  name: 'App',
  components: {
    InputForm,
    TodoList
  },
  data () {
    const todoList = [
      {
        isDone: false,
        text: '薬'
      },
      {
        isDone: true,
        text: 'メモ'
      }
    ];
    return {
      socket : io('localhost:4000'),
      task: '',
      // idを順番につける
      todoList: todoList.map((item, index) => ({ ...item, id: index })),
      // 次のTODOに降るID番号
      nextTodoId: todoList.length
    }
  },
  methods: {
    /**
     * Enterボタンを押したとき
     */
    onSubmit_socketio(e) {
      e.preventDefault();
      this.socket.emit('send', this.$data.text);
    },
    addTask_socketio(text) {
      this.socket.emit('addtask', text);
    },
    /**
     * TODOのチェック
     * @param {number} todoId - TODOのID
     */
    onCheckTodo(todoId) {
      const todo = _.find(this.$data.todoList, { id: todoId });
      if (todo) {
        todo.isDone = !todo.isDone;
      }
    },
    /**
     * TODOの削除ボタンがクリックされた時
     * @param {number} todoId - TODOのID
     */
    onDeleteTodo(todoId) {
      const index = _.findIndex(this.$data.todoList, { id: todoId });
      if (index !== -1) {
        this.$data.todoList.splice(index, 1);
      }
    }
  }
  ,
  created() {
    this.socket.on('connect', () => {
      console.log('connected!');
    });

    /**
     * 送信された時
     * @param {string} task - テキスト
     */
    this.socket.on('addtask', (task) => {
      console.log(task);
        this.$data.todoList.unshift({
        id: this.$data.nextTodoId,
        isDone: false,
        text: task
      });
      this.$data.nextTodoId += 1;
    });
  }
}
</script>

<style>
* {
  box-sizing: border-box;
  outline: none;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
body {
  min-width: 920px;
  margin: 0;
}
.inner {
  width: 920px;
  margin: 0 auto;
}
header {
  background-color: #f7fbff;
  box-shadow: 0 0 4px #b9b9b9;
  margin: 0 0 20px;
  padding: 20px 6px;
}
header h1 {
  font-size: 26px;
  padding-left: 12px;
  display: flex;
  align-items: center;
}
footer {
  background-color: #e7e7e7;
  margin-top: 20px;
  padding: 12px 6px;
}
footer .copy {
  font-size: 18px;
}

</style>
