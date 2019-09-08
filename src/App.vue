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

    <v-content v-if="!loading">
      <v-container>
        <h1>Add Todos</h1>
        <p>Click the task to mark it as complete</p>
        <v-layout
          text-center
          wrap
          row 
          child-flex
          class="container-grid"
        >
          <v-card class="todo-card">
            <InputForm 
              v-on:add-todo="addTodo"
            />
            <TodoList
              v-if="todos.length && !filtered"
              :todos="todos"
              v-on:delete="deleteTodo"
              v-on:complete-todo="completeTodo"
            />
            <TodoList 
              v-if="todos.length && filtered"
              :todos="filteredTodos"
              v-on:delete="deleteTodo"
              v-on:complete-todo="completeTodo"
            ></TodoList>
          </v-card>

          <v-card
            height="150"  
          >
            <h4>Filters</h4>
            <button 
              class="button filter-button"
              v-bind:class="{filterHighlight: this.filtered === null}"
              @click="setFilter(null)"
            >All</button>
            <button 
              class="button filter-button"
              v-bind:class="{filterHighlight: this.filtered === 'complete'}"
              @click="setFilter('complete')"
            >Completed</button>
            <button 
              class="button filter-button"
              v-bind:class="{filterHighlight: this.filtered === 'incomplete'}"
              @click="setFilter('incomplete')"
            >Incomplete</button>
            <v-layout>
              <button 
                class="button remove-button"
                v-if="todos.some(todo => todo.completed)"
                @click="this.deleteCompleted"
              >Remove Completed Tasks</button>
            </v-layout>
          </v-card>

        </v-layout>
      </v-container>
    </v-content>
    <h2 class="loading" v-if="loading">Loading</h2>
    
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
    },
    setFilter(filtered) {
      if (filtered === this.filtered) {
        this.filtered = null;
      } else {
        this.filtered = filtered;
      }
    },
    deleteCompleted () {
      this.todos = this.todos.filter(todo => {
        return !todo.completed;
      });
    }
  },
  mounted: function () {
    let storedTodos = window.localStorage.getItem('todos');
    if (storedTodos) {
      this.todos = JSON.parse(storedTodos);
    }
    this.loading = false;
  },
  watch: {
    todos: {
      handler: function () {
        window.localStorage.setItem('todos', JSON.stringify(this.todos));
      },
      deep: true
    }
  },
  computed: {
    filteredTodos: function () {
      if (this.filtered === 'incomplete') {
        return this.todos.filter((todo) => {
          return !todo.completed;
        });
      } else if (this.filtered === 'complete') {
        return this.todos.filter((todo) => {
          return todo.completed;
        });
      } else {
        return [];
      }
    }
  }
};
</script>
<style>
.button {
  margin: 0 5px;
  font-size: 1rem;
  padding: 5px 10px;
  border-radius: 3px;
}
.filter-button {
  background-color: rgb(182, 176, 176);
  color: white;
}
.filterHighlight {
  background: rgb(93, 228, 93);
}

.remove-button {
  background-color: rgb(202, 22, 22);
  color: white;
  margin: 10px auto;
}

.loading {
  margin: 100px auto;

}

@media only screen and (max-width: 606px) {
    .remove-button {
      margin: 10px auto;
    }
}


</style>