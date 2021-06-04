<template>
  <h1>Vue 3 todo app</h1>
  <form @submit.prevent="addNewTodo">
    <label>New todo</label>
    <input v-model="todoState.newTodo" name="newTodo" />
    <button>Add new todo</button>
  </form>

  <button @click="markAllDone" >Mark all done</button>
  <button @click="removeAll" >Remove all</button>
  <h2>Todos</h2>
  <ul v-for="todo in todoState.todos" :key="todo.id">
    <li class="todo">
      <h3 @click="toggleDone(todo)" :class="{ done: todo.done }">{{ todo.content }}</h3>
      <button @click="removeTodo(todo.id)">Remove todo</button>
    </li>
  </ul>
</template>

<script>
import { reactive } from "vue";

export default {
  setup() {
    const initValue = 'My todo';
    const todoState = reactive({
      newTodo: initValue,
      todos: [],
    });

    function addNewTodo() {
      if (todoState.newTodo) {
        todoState.todos = [
          ...todoState.todos,
          {
            id: Date.now(),
            done: false,
            content: todoState.newTodo,
          },
        ];
        todoState.newTodo = "";
      }
    }

    function toggleDone(todo) {
      todo.done = !todo.done;
    }

    function removeTodo(id) {
      todoState.todos = todoState.todos.filter((todo) => todo.id !== id);
    }

    function markAllDone() {
       todoState.todos = todoState.todos.map((todo) => ({ ...todo, done: true }));
    }

    function removeAll() {
      todoState.todos = []
    }

    return {
      addNewTodo,
      toggleDone,
      removeTodo,
      markAllDone,
      removeAll,
      todoState,
    };
  },
};
</script>

<style>
body {
  font-family: sans-serif;
  padding-top: 1em;
  padding-bottom: 1em;
  font-size: 2em;
  width: 80%;
  margin: 0 auto;
}

input,
textarea,
button,
p,
div,
section,
article,
select {
  display: block;
  width: 100%;
  font-family: sans-serif;
  font-size: 1em;
  margin: 0.5em;
}

.todo {
  cursor: pointer;
}

.done {
  text-decoration: line-through;
}
</style>
