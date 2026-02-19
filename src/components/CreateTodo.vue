<script setup>
import { ref } from 'vue'

const emit = defineEmits(['todo-saved'])
const title = ref("")
const description = ref("")

function saveTodo()
{
    emit("todo-saved", title.value, description.value)

    title.value="";
    description.value="";
}
</script>
<template>
    <div>
        <label for="txtTitle">Title</label>
        <input id="txtTitle" type="text" style="width:100%" v-model="title" />
    </div>
    <div>
        <label for="txtDescription">Description</label>
        <textarea id="txtDescription" style="width:100%;height:150px" v-model="description" />
    </div><br>
    <button @click="saveTodo" value="">Save todo</button>
</template>

<style scoped>
label {
    display:block;
    width:200px;
    font-size:80%;
    color:gray;
    text-transform:uppercase;
}
</style>