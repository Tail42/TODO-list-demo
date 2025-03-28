<template>
    <div class="todo-container">
        <h1>Todo List</h1>
        <div class="input-section">
            <input v-model="newTodo" placeholder="輸入待辦事項" @keyup.enter="addTodo">
            <input v-model="newDueDate" type="date">
            <button @click="addTodo">新增</button>
        </div>

        <ul class="todo-list">
            <TodoItem v-for="(todo, index) in todos" 
            :key="index" :todo="todo" :index="index" 
            @remove="removeTodo" @edit="openEditMenu"
            />
        </ul>
    </div>
  <div v-if="showMenu" class="edit-menu">
    <div class="menu-content">
      <h2>編輯待辦事項</h2>
      <input v-model="editTodo.text" placeholder="待辦事項名稱">
      <input v-model="editTodo.dueDate" type="date">
      <div class="menu-buttons">
        <button @click="saveEdit">儲存</button>
        <button id="delete-button" @click="closeMenu">關閉</button>
      </div>
    </div>

    <div v-if="errorMessage" class="error">{{ errorMessage }}</div>
  </div>
</template>


<script setup>
import { compile, watch, ref } from 'vue';
import TodoItem from './TodoItem.vue';

const newTodo = ref('')
const todos = ref(JSON.parse(localStorage.getItem('todos')) || [])
const newDueDate = ref('')

const showMenu = ref(false)
const editTodo = ref(null)
const editIndex = ref(null)
const errorMessage = ref('') // 未來可能會重複利用到的錯誤訊息

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
    }
}

const removeTodo = (index) => {
    todos.value.splice(index, 1)
}

const openEditMenu = (index) => {
  showMenu.value = true
  editIndex.value = index
  editTodo.value = { ...todos.value[index] }
  
}

const saveEdit = () => {
  showMenu.value = true
  if(editIndex.value !== null && editTodo.value) {  
    if(!editTodo.value.text.trim()) {
      errorMessage.value = '你倒是填啊...'
      return 
    }
    errorMessage.value = ''
    todos.value[editIndex.value] = { ...editTodo.value }
    closeMenu()
  }
}

const closeMenu = () => {
  showMenu.value = false
  editTodo.value = null
  editIndex.value = null
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

#delete-button {
  padding: 8px 16px;
  background-color: #b94279;
  color: white;
  border: none;
  cursor: pointer;
}

#delete-button:hover {
  padding: 8px 16px;
  background-color: #af2164;
  color: white;
  border: none;
  cursor: pointer;
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

.error {
    color: red;
    margin-bottom: 10px;
}
</style>