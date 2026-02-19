<script setup>
import { ref } from 'vue'
import CreateTodo from '@/components/CreateTodo.vue';
import TodoList from '@/components/TodoList.vue';
const todoData = ref([]);
function saveTodo(title, description)
{
    todoData.value.push({
        id: todoData.value.length+1,
        title: title, 
        description : description,
        complete: false})
}
function toggleComplete(id) {
    for (const todo of todoData.value) {
        if (todo.id == id) {
            todo.complete = !todo.complete
        }
    }
}
</script>
<template>
    <div class="content">
        <h1>Todo App</h1>
        <div class="row">
            <div class="col-8">
                <TodoList :todoList="todoData" @toggleComplete="toggleComplete" />
            </div>
            <div class="col-4">
                <CreateTodo @todo-saved="saveTodo"/>
            </div>
        </div>
    </div>
</template>