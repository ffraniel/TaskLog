<template>
  <ul>
    <li v-for="(todo) in todos" :key="todo.id" >
        <p v-bind:class="{complete: todo.completed}">
          <span @click="$emit('complete-todo', todo.id)">{{todo.todo}}</span>
          
          <input 
            type="checkbox" 
            v-model="todo.completed"
          />
          <v-btn 
            name="delete" 
            @click="$emit('delete', todo.id)"
            color="rgb(108, 204, 204)"
          >x</v-btn>
        </p>
    </li>
  </ul>
</template>
<script>

export default {
  name: 'TodoList',
  props: {
    todos: Array
  }
}</script>
<style scoped>
  ul {
    list-style: none;
  }
  ul p {
    font-size: 1.75rem;
  }
  p input {
    margin: 0 20px;
  }
  button {
    background-color: rgb(179, 245, 245);
    font-size: 1.75rem;
    width: 2.75rem;
    border-radius: 5px;
    transition: background-color 0.25s;
  }
  button:hover {
    background-color: rgb(148, 96, 96);
  }
  span {
    margin: 0 20px;
  }
  .complete {
    text-decoration: line-through;
  }
</style>