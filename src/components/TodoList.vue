<script setup>
const props = defineProps(["todoList"])
const emits = defineEmits(["toggleComplete"])
function toggleComplete(id)
{
    emits("toggleComplete", id)
}
</script>
<template>
<div :class="{complete: todo.complete}" class="todo" v-for="todo in props.todoList" @click="toggleComplete(todo._id)">
    <div class="title">{{ todo.title }}</div>
    <div style="color:gray">{{ todo.description }}</div>    
</div>
</template>
<style scoped>
.todo {
    margin-bottom:30px;
    cursor:pointer;
}
.todo .title {
    font-weight:bold;
    font-size:120%;
}
.complete {
    text-decoration:line-through;
}
</style>