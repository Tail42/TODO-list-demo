<template>
    <div class="todo-container">
        <h1>Todo List</h1>
        <div class="input-section">
            <input v-model="newTodo" placeholder="輸入待辦事項" @keyup.enter="addTodo">
            <button @click="addTodo">新增</button>
        </div>

        <ul class="todo-list">
            <TodoItem v-for="(todo, index) in todos" :key="index" :todo="todo" :index="index" @remove="removeTodo" />
        </ul>
    </div>
</template>


<script setup>
import { compile, ref } from 'vue';
import TodoItem from './TodoItem.vue';

const newTodo = ref('')
const todos = ref([])

const addTodo = () => {
    if (newTodo.value.trim()) {
        todos.value.push({
            text: newTodo.value,
            completed: false
        })
        newTodo.value = ''
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