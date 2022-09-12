<template>
  <div class="box">
    <myheader :todos="todos" @addTodo="addTodo" />
    <mylist :todos="todos" />
    <myfooter :todos="todos" @checkedAllTodos="checkedAllTodos" @deleteTodos="deleteTodos" />
  </div>
</template>

<script>
import myheader from "./components/MyHeader";
import mylist from "./components/MyList";
import myfooter from "./components/MyFooter";
import { nanoid } from "nanoid";
export default {
  name: "App",
  data() {
    return {
      todos: JSON.parse(localStorage.getItem("todos")) || []
    };
  },
  components: { myheader, myfooter, mylist },
  methods: {
    addTodo(name) {
      this.todos.unshift({
        id: nanoid(),
        name: name,
        done: false
      });
    },
    deleteTodo(id) {
      this.todos = this.todos.filter(todo => {
        return todo.id !== id;
      });
    },
    changeTodo(id) {
      this.todos.forEach(todo => {
        if (todo.id == id) todo.done = !todo.done;
      });
    },
    checkedAllTodos(done) {
      this.todos.forEach(todo => {
        todo.done = done;
      });
    },
    deleteTodos() {
      this.todos = this.todos.filter(todo => {
        return !todo.done;
      });
    },
    updateTodo(id, name) {
      this.todos.forEach(todo => {
        if (todo.id == id) todo.name = name;
      });
    }
  },
  watch: {
    todos: {
      deep: true,
      handler(value) {
        localStorage.setItem("todos", JSON.stringify(value));
      }
    }
  },
  mounted() {
    this.$bus.$on("deleteTodo", this.deleteTodo);
    this.$bus.$on("changeTodo", this.changeTodo);
    this.$bus.$on("updateTodo", this.updateTodo);
  },
  beforeDestroy() {
    this.$bus.$off("deleteTodo");
    this.$bus.$off("changeTodo");
    this.$bus.$off("updateTodo");
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  font-size: 13px;
}
.box {
  margin: 10px auto;
  width: 500px;
  border: 1px solid black;
  /* box-shadow: 3px 3px silver; */
  padding: 15px;
}
</style>