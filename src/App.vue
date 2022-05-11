<script setup>
  //import HelloWorld from './components/HelloWorld.vue'
  //import TheWelcome from './components/TheWelcome.vue'

  import { ref, computed } from 'vue'

  const hideCompleteTodos = ref(true)

  let id = 0

  const alert = ref('')
  const newTodo = ref('')
  const todos = ref([
    { id: id++, text: 'Add some todos!', done: false }
  ])

  function addTodo() {
    if (newTodo.value !== "") {
      todos.value.push({
        id: id++,
        text: newTodo.value
      })
      newTodo.value = ''
    } else {
      alert.value = "Please add some context for your new todo!"
    }
  }

  function resetAlert() {
    alert.value = ''
  }

  function removeTodo(todo) {
    todos.value = todos.value.filter(t => t !== todo)
  }

  const filteredTodos = computed(() => hideCompleteTodos.value
    ? todos.value.filter(t => !t.done)
    : todos.value
  )

  function toggleTodoView () {
    hideCompleteTodos.value = !hideCompleteTodos.value
  }

</script>

<template>
  <div id="todo-container">
    <div id="todo-contents">
      <button id="todo-toggle" @click="toggleTodoView">
        {{ hideCompleteTodos ? "< TODO >" : "< TODO />"}}
      </button>
      <form @submit.prevent="addTodo">
        <div id="todo-form">
          <input type="text" @click="resetAlert" v-model="newTodo" placeholder="Enter a new todo...">
          <button>+</button>
        </div>
      </form>
      <span class="form-alert">{{ alert }}</span>
      <div id="todo-list">
        <div v-for="todo in filteredTodos" :key="todo.id" class="todo-item">
          <input type="checkbox" v-model="todo.done">
          <span :class="{ done: todo.done }">{{ todo.text }}</span>
          <button class="delete-todo" @click="removeTodo(todo)">x</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
  #todo-container {
    height: 100vh;
    width: 100vw;
    display: grid;
    align-items: center;
  }

  #todo-contents {
    min-width: 250px;
    margin: 0 auto;
    background: #ffffff;
    border-radius: 5px;
    padding: 2rem;
  }

  #todo-contents form {
    display: block;
    width: 100%;
  }

  #todo-form {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  #todo-form button {
    background: white;
    border: 1px solid green;
    text-align: center;
    color: green;
    height: 25px;
    width: 25px;
    border-radius: 100%;
    font-weight: bold;
  }

  #todo-form button:hover {
    color: white;
    background: green;
  }

  #todo-contents input[type="text"] {
    padding: 0.5rem 1rem;
    border-radius: 50px;
    border: 1px solid #eee;
  }

  #todo-contents input[type="checkbox"] {
    margin-right: 0.5rem;
  }

  .form-alert {
    color: red;
    font-size: 0.8rem;
    display: block;
    text-align: center;
    margin-top: 5px;
  }

  #todo-list {
    margin: 1rem auto;
  }

  button.delete-todo {
    text-align: center;
    color: red;
    background: transparent;
    border: none;
  }

  button.delete-todo:hover {
    font-weight: bold;
  }

  .todo-item {
    list-style: none;
    padding: 0;
    margin: 0;
  }

  .done {
    text-decoration: line-through;
  }

  #todo-toggle {
    display: block;
    margin: 0 auto 1rem auto;
    padding: 0.5rem 1rem;
    border-radius: 50px;
    border: 1px solid #eee;
    background: transparent;
    width: 150px;
    font-family: monospace;
  }

  #todo-toggle:hover {
    background: #eee;
  }
</style>
