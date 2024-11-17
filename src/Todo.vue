<template>
  <div class="app-container">
    <!-- Title -->
    <div class="col-12 header-container">
      <div class="col-12 title-container">
        <label class="title-label">ToDo App</label>
      </div>
    </div>

    <!-- Add-ToDo section -->
    <div class="col-5 add-todo-container">
      <label class="add-todo-label">New ToDo</label>
      <input type="text" class="add-todo-field form-control" v-model="newTodo" name="newTodo" autocomplete="off" />
      <button class="add-button" @click="addTodo()">Add ToDo</button>
    </div>

    <!-- List Section -->
    <div class="col-5 todo-list-container">
      <div class="col-12 todo-list-title-label-container">
        <label class="todo-list-label">ToDo List</label>
      </div>

      <hr class="col-12">

      <ul class="col-12 todo-list">
        <li v-for="(todo, index) in todos" :key="index" class="col-12 todo-item">
          <div class="todo-content">
            <span id="todo-content-label" :class="{ done: todo.done }" @click="doneTodo(todo)">{{ todo.content }}</span>
          </div>

          <div class="remove-button-container">
            <button id="todo-content-remove-button" @click="removeTodo(index)">Remove</button>
          </div>
        </li>
      </ul>

      <div v-if="todos.length === 0" class="empty-message">Empty list. Add smthng new!</div>
    </div>
  </div>
</template>
  

<script>
import { ref } from 'vue'
export default {
  name: 'TodoScreen',
  setup() {
    const newTodo = ref('')
    const defaultData = [
      {
        done: false,
        content: 'Welcome! Start by adding smthng new!',
      },
    ]
    const todosData = JSON.parse(localStorage.getItem('todos')) || defaultData
    const todos = ref(todosData)
    function addTodo() {
      if (newTodo.value) {
        todos.value.push({
          done: false,
          content: newTodo.value,
        })
        newTodo.value = ''
      }
      saveData()
    }

    function doneTodo(todo) {
      todo.done = !todo.done
      saveData()
    }

    function removeTodo(index) {
      todos.value.splice(index, 1)
      saveData()
    }

    function saveData() {
      const storageData = JSON.stringify(todos.value)
      localStorage.setItem('todos', storageData)
    }

    return {
      todos,
      newTodo,
      addTodo,
      doneTodo,
      removeTodo,
      saveData,
    }
  },
}
</script>

<style scoped>

.app-container {
    display: flex;
    flex-direction: column;
    background: #18141a;
    align-items: center;
    min-height: 100vh;
    height: auto;
}

.header-container {
    margin-top: 30px;;
    display: flex;
    flex-direction: row;
    height: 10%;
}

.title-container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}


.title-label {
    color: #FFF;
    font-family: "Instrument Sans";
    font-size: 50px;
    font-style: normal;
    font-weight: 700;
    line-height: normal;
}

hr {
    border-top: 2px solid rgb(255, 255, 255);
}

.add-todo-container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    height: 20%;
}

.add-todo-label {
    color: #FFF;
    font-family: "Instrument Sans";
    font-size: 20px;
    font-weight: 700;
    line-height: normal;
}

.add-todo-field {
    border-radius: 5px;
    font-family: "Instrument Sans";
    font-size: 20px;
    color: white;
    border: 2px solid rgba(223, 223, 223, 0.27);
    background: transparent;
    height: 60px;
    align-self: center;
}

.add-button {
    align-self: center;
    margin-top: 25px;
    border-radius: 5px;
    border: 1px solid rgba(0, 0, 0, 0.27);
    font-size: 25px;
    font-weight: 700;
    background: #9F84A9;
    width: 100%;
    height: 30%;
}

.todo-list-container {
    margin-top: 10px;
    display: flex;
    flex-direction: column;
    height: 70%;
}

.todo-list-title-label-container {
    display: flex;
    flex-direction: column;
    color: #FFF;
    font-family: "Instrument Sans";
    font-size: 20px;
    font-weight: 700;
    line-height: normal;
}

.todo-list-label {
    color: white;
    align-self: flex-start;
}

.list-title-label {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

.todo-item {
    display: flex;
    margin-top: 25px;
    flex-direction: row;
    border-radius: 5px;
    border: 2px solid rgba(223, 223, 223, 0.27);
    background: transparent;
    height: 60px;
}

.todo-content {
    display: flex;
    flex-direction: row;
    width: 80%;
}

.remove-button-container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-content: center;
    width: 20%;
}

#todo-content-label {
    color: white;
    align-self: center;
    margin-left: 10px;
    font-size: 18px;
}

.done {
    text-decoration: line-through;
}

#todo-content-remove-button {
    border-radius: 5px;
    border: 1px solid rgba(0, 0, 0, 0.27);
    background: #9F84A9;
    height: 55%;
    align-self: center;
    font-weight: 500;
}

.empty-message {
    color: #FFF;
    font-family: "Instrument Sans";
    font-size: 20px;
    font-weight: 700;
    line-height: normal;
}
</style>