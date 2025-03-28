<template>
    <li class="todo-item" :class="{ overdue: isOverdue(todo) }">
        <input 
            type="checkbox" 
            v-model="todo.completed"
        >
        <span :class="{ completed: todo.completed }">{{ todo.text }}</span>
        <small v-if="todo.dueDate">截止日期: {{ todo.dueDate }}</small>
        <button @click="emit('remove', 'index')">刪除</button>
    </li>
</template>

<script setup>
defineProps({
    todo:{
        type:Object,
        required: true
    },
    index: {
        type: Number,
        required: true
    }
})

const emit = defineEmits(['remove'])

const isOverdue = (todo) => {
    if(!todo.dueDate || todo.completed) return false
    const due = new Date(todo.dueDate)
    const today = new Date()
    return (due < today)
}

</script>

<style scoped>
.todo-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px;
  border-bottom: 1px solid #eee;
}

.completed {
  text-decoration: line-through;
  color: #888;
}

.overdue {
  background-color: #ffe6e6; /* 過期時的背景色 */
  color: #d32f2f; /* 過期時的文字顏色 */
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

small {
  color: #666;
  margin-left: 5px;
}
</style>