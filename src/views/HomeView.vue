<script setup>
import { ref, onMounted } from 'vue'
import CreateTodo from '@/components/CreateTodo.vue';
import TodoList from '@/components/TodoList.vue';
//This is a PouchDB class
import PouchDB from 'pouchdb';

const todoData = ref([])
async function saveTodo(todoDocument)
{
    if (todoDocument._id == undefined)
        todoDocument._id =  new Date().getTime()+"";
    const database = new PouchDB("todoApp")
    try {
        await database.put(todoDocument);
        todoData.value = await loadTodos()    
    }
    catch(e) {
        console.error(e)
    }
}
async function loadTodos()
{
    //This represents a database instance
    const database = new PouchDB("todoApp")
    
    var allDocs = await database.allDocs();
    const todos = []
    
    if (allDocs.total_rows != 0) {
        for (let todoId of allDocs.rows) {
            const todoDoc = await database.get(todoId.id);
            todos.push(todoDoc);
        }
    }
    return todos;
}

onMounted(async () => {
    try
    {
        todoData.value = await loadTodos()
    } 
    catch (error)
    {
        console.error("Could not load documents")
    }
})

function toggleComplete(id) {
    for (const todo of todoData.value) {
        if (todo._id == id) {
            todo.complete = !todo.complete
            saveTodo(todo);
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