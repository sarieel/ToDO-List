<template>
  <transition name="todo" appear>
    <p>
      <input
        class="check"
        type="checkbox"
        :checked="todo.done"
        @click="change(todo.id)"
      />
      <span class="editText" v-show="!todo.isEdit">{{ todo.name }}</span>
      <input
        class="editText"
        ref="editInput"
        type="text"
        v-show="todo.isEdit"
        :value="todo.name"
        @blur="update(todo, $event)"
        @keydown.enter="update(todo, $event)"
      />
      <button @click="del(todo.id)">删除</button>
      <button class="edit" @click="edit(todo)">编辑</button>
    </p>
  </transition>
</template>

<script>
export default {
  name: "MyItem",
  props: ["todo"],
  methods: {
    del(id) {
      this.$bus.$emit("deleteTodo", id);
    },
    change(id) {
      this.$bus.$emit("changeTodo", id);
    },
    edit(todo) {
      this.$set(todo, "isEdit", true);
      this.$nextTick(function () {
        this.$refs.editInput.focus();
      });
    },
    update(todo, e) {
      this.$bus.$emit("updateTodo", todo.id, e.target.value);
      todo.isEdit = false;
    },
  },
};
</script>

<style scoped>
.todo-enter,
.todo-leave-to {
  transform: translateX(100%);
}
.todo-leave,
.todo-enter-to {
  transform: translateX(0%);
}
.todo-enter-active,
.todo-leave-active {
  transition: 0.1s linear;
}
p {
  position: relative;
  height: 40px;
  border: 1px solid silver;
  border-bottom: 0;
}
.check {
  margin: 5px 10px;
  height: 30px;
}
p button {
  display: none;
  float: right;
  margin-top: 7.5px;
  margin-right: 10px;
  width: 45px;
  height: 25px;
  border: 0;
  color: white;
  background-color: red;
}
.editText {
  position: absolute;
  top: 10.75px;
}
p:hover {
  background-color: rgb(190, 190, 190);
}
p:hover button {
  display: inline-block;
}
.edit {
  background-color: skyblue;
}
</style>
