<template>
<transition-group tag="ul" class="taskList" name="flip">
    <li
        v-for="item in $props.todoList"
        :key="item.id"
        class="item"
        @click="$emit('check', item.id)"
    >
        <input type="checkbox" :checked="item.isDone" />
        <span class="item__text">{{ item.text }}</span>
        <button @click="onDeleteTodo($event, item.id)">削除</button>
    </li>
</transition-group>
</template>

<script>
export default {
    props: {
      todoList: {
      id: Number,
      isDone: Boolean,
      text: String
    }
    },
  methods: {
    /**
     * 削除ボタンをクリックした時
     * @param {event} event - DOMのイベント
     * @param {number} todoId - TODOのID
     */
    onDeleteTodo(event, todoId) {
      event.stopPropagation();
      this.$emit('delete', todoId);
    }
}
}
</script>

<style scoped>
.taskList {
    margin: 40px 0 0;
    padding: 0;
    text-align: left;
}
 .taskList > li {
     position: relative;
     display: flex;
     align-items: center;
     padding: 0;
 }
 .taskList > li + li {
     margin-top: 16px;
 }
 .taskList > li > input {
     margin-right: 10px;
     border: 0;
 }
 .taskList > li > button {
     position: absolute;
     color: #333333;
     background-color: #ffebeb;
     border: 1px solid #ffbaba;
     border-radius: 20px;
     top: 50%;
     right: 0;
     width: 120px;
     margin-top: -12px;
     padding: 2px 10px;
 }
 .taskList > li > button:hover,
 .taskList > li > button:active {
     background-color: #ffebeb;
     border: 2px solid #ffb4b4;
 }
</style>