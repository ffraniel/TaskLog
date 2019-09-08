<template>
  <v-app>

    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>Task Log</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        text
        href="https://github.com/ffraniel"
        target="_blank"
      >Francis Whitehead
      </v-btn>
    </v-app-bar>

    <v-content>
      <v-container>
        <h1>Add Todos</h1>
        <p>Click the task to mark it as complete</p>
        <v-layout
          text-center
          wrap
        >
          <v-card
            min-width="800"
            class="mx-auto"
          >
            <InputForm 
              v-on:add-todo="addTodo"
            />
            <TodoList
              v-if="todos.length && !filtered"
              :todos="todos"
              v-on:delete="deleteTodo"
              v-on:complete-todo="completeTodo"
            />
        

          </v-card>

        </v-layout>
      </v-container>
    </v-content>
  
  </v-app>
</template>

<script>
import uuid from 'uuid';
import InputForm from './components/InputForm.vue';
import TodoList from './components/TodoList.vue';

export default {
  name: 'App',
  components: {
    InputForm,
    TodoList
  },
  data: () => ({
    todos: [],
    loading: true,
    filtered: null
  }),
  methods: {
    addTodo (todo) {
      let input = todo.trim();
      if (input.length < 1) {
        return;
      }
      let newTodo = {
        todo,
        completed: false,
        id: uuid.v4()
      };
      this.todos.push(newTodo);
    },
    deleteTodo (id) {
      this.todos = this.todos.filter(item => id !== item.id);
    },
    completeTodo (id) {
      this.todos = this.todos.map(todo => {
        if (todo.id === id) {
          let updatedTodo = todo;
          updatedTodo.completed = !updatedTodo.completed;
          return updatedTodo;
        } else {
          return todo;
        }
      });
    }
  }
};
</script>
<style>
</style>