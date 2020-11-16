<template>
  <form @submit="componentSubmit" class="taskForm">
      <input v-model="$data.task" type="text" placeholder="タスクを入力してください" />
      <button type="submit" :disabled="$data.task === ''">送信</button>
    </form>
</template>

<script>

export default {
  name: 'MyComponent',
  data () {
    return {
      task: ''
    }
  },
  methods: {
     componentSubmit(event) {
      event.preventDefault();
      this.$emit('onsubmit', this.$data.task);
      this.$data.task = '';
    }
  }
}
</script>

<style scoped>
.taskForm {
  display: flex;
  justify-content: space-between;
}
.taskForm > input + button {
  margin-left: 10px;
  width: 120px;
}
.taskForm > input {
  width: 80%;
  border: 1px solid rgb(170, 170, 170);
  border-radius: 4px;
  padding: 8px 16px;
}
</style>