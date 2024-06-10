<template>
    <div class="todo-list">
      <h1>Todo List</h1>
      <input v-model="newTodo" placeholder="Add a new task" @keyup.enter="addTodo" />
      <button @click="addTodo">Add</button>
      <ul>
        <li v-for="(todo, index) in todos" :key="index">
          <span :class="{ completed: todo.completed }" @click="toggleTodoCompletion(index)">
            {{ todo.text }}
          </span>
          <button @click="deleteTodo(index)">Delete</button>
        </li>
      </ul>
      <p>{{ incompleteTodos }} tasks remaining</p>
    </div>
  </template>
  
  <script>
  import { useTodoStore } from '../stores/taskStore';
  import { computed, ref } from 'vue';
  
  export default {
    setup() {
      const store = useTodoStore();
      const newTodo = ref('');
  
      const addTodo = () => {
        if (newTodo.value.trim()) {
          store.addTask(newTodo.value);
          newTodo.value = '';
        }
      };
  
      const deleteTodo = (index) => {
        store.removeTask(index);
      };
  
      const toggleTodoCompletion = (index) => {
        store.toggleTaskStatus(index);
      };
  
      return {
        newTodo,
        todos: computed(() => store.tasks),
        addTodo,
        deleteTodo,
        toggleTodoCompletion,
        incompleteTodos: computed(() => store.incompleteTasksCount),
      };
    },
  };
  </script>
  
  <style scoped>
  .todo-list {
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f9f9f9;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

input {
  width: calc(100% - 22px);
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
}

button {
  padding: 10px;
  margin-left: 5px;
  background-color: #28a745;
  color: #fff;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #218838;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #eee;
}

li.completed span {
  text-decoration: line-through;
  color: #888;
}

li span {
  cursor: pointer;
}

p {
  text-align: right;
  font-weight: bold;
}
</style>
