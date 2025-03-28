<template>
    <div class="todo-container">
        <h1>Todo List</h1>
        <div class="input-section">
            <input v-model="newTodo" placeholder="輸入待辦事項" @keyup.enter="addTodo">
            <input v-model="newDueDate" type="date">
            <button @click="addTodo">新增</button>
        </div>

        <ul class="todo-list">
            <TodoItem v-for="(todo, index) in todos" :key="index" :todo="todo" :index="index" @remove="removeTodo" />
        </ul>
    </div>
</template>


<script setup>
import { compile, watch, ref } from 'vue';
import TodoItem from './TodoItem.vue';

const newTodo = ref('')
const todos = ref(JSON.parse(localStorage.getItem('todos')) || [])

const newDueDate = ref('')

const saveTodos = () => {
  localStorage.setItem('todos', JSON.stringify(todos.value))
}

// 監聽 todos 的變化
watch(todos, () => {
  saveTodos()
}, { deep: true }) // deep: true 確保監聽陣列內物件的變化

const addTodo = () => {
    if (newTodo.value.trim()) {
        todos.value.push({
            text: newTodo.value,
            completed: false,
            dueDate: newDueDate.value || null
        })
        newTodo.value = ''
        newDueDate.value = ''
        // saveTodos()
    }
}

const removeTodo = (index) => {
    todos.value.splice(index, 1)
}
</script>

<style scoped>
.todo-container {
  max-width: 600px;
  margin: 20px auto;
  padding: 20px;
}

.input-section {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

input[type="text"] {
  flex: 1;
  padding: 8px;
}

button {
  padding: 8px 16px;
  background-color: #42b983;
  color: white;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #359c6d;
}


</style>