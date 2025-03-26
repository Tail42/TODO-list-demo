<template>
    <li class="todo-item">
        <input 
            type="checkbox" 
            v-model="todo.completed"
        >
        <span :class="{ completed: todo.completed }">{{ todo.text }}</span>
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
</script>

<style scoped>

</style>