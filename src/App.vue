<template>
  <div id="app">
    <div class="container" v-if="todos">
      <h1 class="header">Todo list</h1>
      <div class="todos-holder">
        <input
          type="text"
          class="new"
          placeholder="Add new todo..."
          v-model="newTodo"
          @keyup="add"
        />
        <p v-if="!todos.length" class="empty">There are no todos yet!</p>
        <Todo
          v-for="todo in todos"
          :todo="todo"
          :key="todo.id"
          @remove="remove"
          @update="update"
        />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Todo from "./components/Todo";

export default {
  name: "App",
  components: {
    Todo
  },
  data() {
    return {
      newTodo: "",
      todos: null
    };
  },
  methods: {
    add(e) {
      if (e.key == "Enter" && this.newTodo.length > 0) {
        axios
          .post("http://localhost:8000/api/todo-create/", {
            text: this.newTodo,
            completed: false
          })
          .then(response => {
            this.todos.unshift(response.data);
            this.newTodo = "";
          })
          .catch(err => console.log(err));
      }
    },
    remove(id) {
      axios
        .delete("http://localhost:8000/api/todo-delete/" + id + "/")
        .then(response => {
          this.todos.splice(
            this.todos.findIndex(todo => todo.id == response.data.id),
            1
          );
        })
        .catch(err => console.log(err));
    },
    update(todo) {
      axios
        .post("http://localhost:8000/api/todo-update/" + todo.id + "/", {
          id: todo.id,
          completed: !todo.completed,
          text: todo.text
        })
        .then(response => {
          this.todos.splice(
            this.todos.findIndex(todo => todo.id == response.data.id),
            1,
            response.data
          );
        })
        .catch(err => console.log(err));
    }
  },
  created() {
    axios
      .get("http://localhost:8000/api/todo-list/")
      .then(response => {
        this.todos = response.data;
      })
      .catch(err => console.log(err));
  }
};
</script>

<style lang="sass">
*
  padding: 0
  margin: 0
  box-sizing: border-box

#app
  font-family: Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  min-height: 100vh
  // background: #F0F0F0
  background-image: linear-gradient(to left bottom, #ffffff, #fbfbfb, #f8f8f8, #f4f4f4, #f1f1f1, #efeff2, #ebedf3, #e6ecf4, #daedf7, #ceeff4, #c6f0e9, #c7efd9)
  display: flex
  justify-content: center
  color: #292929

.container
  display: flex
  flex-direction: column
  align-items: center
  width: 70vw
  max-width: 700px
  min-width: 500px

.header
  margin: 60px 0 15px 40px
  align-self: flex-start
  font-size: 60px

.todos-holder
  display: flex
  flex-direction: column
  align-items: flex-start
  padding: 40px 40px 25px 40px
  border: 1px solid #E9E9E9
  border-radius: 8px
  background-color: white
  box-shadow: 3px 3px 3px #E0E0E0
  width: 100%
  margin-bottom: 60px

.new
  border: none
  outline: none
  padding: 0px 25px 15px 10px
  margin-bottom: 15px
  cursor: pointer
  font-weight: 700
  font-size: 20px

.empty
  font-weight: 700
  font-size: 20px
  padding-left: 10px
</style>
