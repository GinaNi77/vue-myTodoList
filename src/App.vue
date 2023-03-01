<template>
<div class="wrapper-todo">
    <div class="title has-text-centered has-text-weight-semibold has-text-primary-dark">My Todo List</div>
    <div>{{todos.length}}</div>
    <form @submit.prevent="addTodo">
         <div class="field is-grouped mb-5">
        <p class="control is-expanded">
            <input class="input" 
            type="text" 
            placeholder="Add a todo" 
            v-model="newtodoContent">
        </p>
        <p class="control">
            <button class="button is-info has-background-primary"
            :disabled="!newtodoContent">Add</button>
        </p>
    </div>
    </form>
    <div v-for="todo in todos" :key="todo.id"
    class="card mb-5" :class="{'has-background-success-light' : todo.done}">
        <div class="card-content">
            <div class="content">
                 <div class="columns is-mobile is-vcentered">
                    <div class="column" :class="{'has-text-success line-through' : todo.done}">{{todo.content}}</div>
                    <div class="column is-5 has-text-right">
                        <button class="button has-text-white mr-2"
                        :class="todo.done ? 'is-success' : 'is-light'" @click="togglerDone(todo.id)">&check;</button>
                        <button class="button has-background-danger has-text-white" @click="deleteTodo(todo.id)">&cross;</button>    
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
</template>

<script setup>
//import
import {ref} from 'vue';
import { v4 as uuidv4 } from 'uuid';

//todos

const todos = ref([
    // {
    //     id:'id1',
    //     content: 'Excuse me, brah',
    //     done: false    
    // },
    // {
    //     id:'id2',
    //     content: 'Youre excused',
    //     done: false    
    // },
    {
        id:'id3',
        content: 'And Im not your brah',
        done: true    
    }
]);

//add todo

const newtodoContent = ref('')

const addTodo = ()=>{
    const newTodo = {
        id: uuidv4(),
        content: newtodoContent.value,
        done: false 
    }
    todos.value.unshift(newTodo)
    newtodoContent.value =""
}
//delete todo

const deleteTodo = (id)=>{
    todos.value = todos.value.filter((todo)=> todo.id!==id)
}

//todo done

const togglerDone = (id)=>{
    const index = todos.value.findIndex(todo => todo.id === id)
    todos.value[index].done = !todos.value[index].done
}
</script>


<style>
@import 'bulma/css/bulma.min.css';


.wrapper-todo{
    padding: 20px;
    max-width: 400px;
    margin: 0 auto;
}

.line-through{
    text-decoration: line-through;
}
</style>