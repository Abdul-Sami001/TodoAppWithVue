<script setup>
import { ref, onMounted, computed, watch } from 'vue'
const todos = ref([]);
const name = ref('');
const input = ref('');
const category = ref(null);
const addTodo = () => {
  if (input.value.trim() === '' || category.value === null) {
    return
  }
  console.log("add todo")
  todos.value.push({
    content: input.value,
    category: category.value,
    createdAt: new Date().getTime(),
    done: false
  })
  input.value = ''
  category.value = null
};
watch(todos, newValue => {
  localStorage.setItem('todos', JSON.stringify(newValue));
}, { deep: true });

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})

const todos_asc = computed(() => todos.value.sort((a, b) => {
  return b.createdAt - a.createdAt
}))

const removeTodo = todo => {
  todos.value = todos.value.filter(t => t !== todo)
}
</script>

<template>
  <div id="app">
    <h1>Welcome, list your tasks</h1>
    <div class="todos">
      <form @submit.prevent="addTodo">
        <h2>Enlist your todos</h2>
        <input type="text" name="" id="" placeholder="Enter your task" v-model="input" />
        <h3>Pick a category</h3>
        <div class="cards">
          <div class="card1">
            <label for="category">Business</label>
            <input type="radio" name="category" value="Business" id="" v-model="category">
          </div>
          <div class="card2">
            <label for="category">Personal</label>
            <input type="radio" name="category" id="" value="Personal" v-model="category">
          </div>
          <button type="submit">Add Todo</button>
        </div>
      </form>
    </div>
    <h3>Todo's list</h3>
    <div class="list" v-for="todo in todos_asc" :key="todo">
      <input type="text" v-model="todo.content">
      <button @click="removeTodo(todo)">Delete</button>
    </div>



  </div>
</template>

<style scoped>
#app {
  width: 40%;
  margin: auto;
}

form {
  display: flex;
  flex-direction: column;
  align-items: center;
}

h2 {
  margin-bottom: 16px;
}

input[type="text"] {
  width: 100%;
  padding: 8px;
  margin-bottom: 16px;
  border-radius: 4px;
  border: 1px solid #ccc;
}

h3 {
  margin-bottom: 8px;
}

.cards {
  display: flex;
  justify-content: space-between;
  margin-bottom: 16px;
}

.card1,
.card2 {
  flex-basis: calc(50% - 8px);
}

.card {
  background-color: #f0f0f0;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  padding: 16px;
}

.card label {
  display: block;
  margin-bottom: 8px;
  font-weight: bold;
}

.card input[type="radio"] {
  margin-bottom: 8px;
}

button[type="submit"] {
  display: block;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  padding: 8px 16px;
  width: 100%;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button[type="submit"]:hover {
  background-color: #0056b3;
}
</style>
