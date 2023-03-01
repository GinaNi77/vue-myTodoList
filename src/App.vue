<template>
    <div class="wrapper-todo">

        <div class="columns is-mobile">
            <div class="column is-half">
                <div class="title has-text-weight-semibold has-text-white">My Todo List</div>
            </div>
            <div class="column">
               
            </div>
            <div class="column is-flex is-justify-content-end">
                <div class="list-lenght">
                    <p>{{todos.length}}</p>
                </div>  
            </div>
        </div>

        <div class="is-flex is-justify-content-center mb-6">
             <span class="icon has-text-white fa-2x"  v-if="editing">
                    <i class="fa-regular fa-rectangle-xmark" @click="doEdit(false)"></i>
                </span>
                <span class="icon has-text-white fa-2x" v-else>
                    <i class="fa-regular fa-pen-to-square" @click="doEdit(true)"></i>
                </span> 
           
            
           
        </div>
        
        
        <form @submit.prevent="addTodo" v-if:="editing">
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
       
        <div v-for="todo in todos"
        :key="todo.id"
        class="card mb-5 card-background-border"
        :class="{'done-border' : todo.done}">
            <div class="card-content">
                <div class="content">
                    <div class="columns is-mobile is-vcentered">
                        
                        <div class="column has-text-white"
                        :class="{'has-text-success line-through' : todo.done}">{{todo.content}}</div>
                        <div class="column is-5 has-text-right">
                            <button class="button has-text-white mr-2"
                            :class="todo.done ? 'is-primary' : 'is-light'"
                            @click="togglerDone(todo.id)">&check;
                            </button>
                            
                            <button class="button has-background-danger has-text-white"
                            @click="deleteTodo(todo.id)">&cross;
                            </button>    
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

const todos = ref([]);

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

//edit list
const editing = ref(false)

const doEdit = (e)=>{
    editing.value = e
    newtodoContent.value = ""

}
</script>


<style>
@import 'bulma/css/bulma.min.css';


.wrapper-todo{
    padding: 20px;
    max-width: 500px;
    margin: 0 auto;
}

.line-through{
    text-decoration: line-through;
}

.card-background-border{
    box-shadow: 
        0 0 1rem hsl(348, 86%, 43%),
        0 0 1rem hsl(348, 86%, 43%),
        0 0 1rem hsl(348, 86%, 43%);
    background-color: rgb(1, 1, 26);
}

.done-border{
    box-shadow: 
        0 0 0.8rem hsl(171, 100%, 29%),
        0 0 1rem hsl(171, 100%, 29%),
        0 0 1rem hsl(171, 100%, 29%); 
}

.list-lenght{
    color: #fff;
    border: 2px solid ;
    border-radius: 50%;
    width: 3em;
    height: 3em;
    display: flex;
    justify-content: center;
    align-items: center;
}
</style>