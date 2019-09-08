<template>
  <v-form
    @submit="formSubmitHandler"
  >
    <input 
      class="todo-input"
      name="todo-input"
      type="text"
      placeholder="Add a todo to Tasklog!"
      v-model="inputText"
    />
  </v-form>
</template>

<script>

export default {
  name: 'InputForm',
  data: () => ({
    inputText: ''
  }),
  methods: {
    formSubmitHandler (e) {
      e.preventDefault();
      this.$emit('add-todo', this.inputText);
      this.inputText = '';
    }
  }
};
</script>
<style>
  .todo-input {
    box-shadow: 1px 2px 10px 3px #e4e2e2;
    font-size: 1.25rem;
    padding: 5px;
    margin: 5px;
  }
</style>