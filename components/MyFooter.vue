<template>
  <div class="foot" v-show="total!=0">
    <input type="checkbox" v-model="isAll" />
    <span>已完成{{finished}} / 全部{{total}}</span>
    <button @click="del">删除已完成选项</button>
  </div>
</template>

<script>
export default {
  name: "MyFoot",
  props: ["todos", "checkedAllTodos", "deleteTodos"],
  methods: {
    del() {
      this.$emit("deleteTodos");
    }
  },
  computed: {
    finished() {
      return this.todos.reduce((pre, current) => {
        return pre + (current.done ? 1 : 0);
      }, 0);
    },
    total() {
      return this.todos.length;
    },
    isAll: {
      get() {
        return this.finished == this.total;
      },
      set(v) {
        this.$emit("checkedAllTodos", v);
        // this.checkedAllTodos(v);
      }
    }
  }
};
</script>

<style scoped>
.foot {
  position: relative;
  margin-top: 15px;
  height: 30px;
}
.foot input {
  margin: 0 10px;
  height: 30px;
}
.foot span {
  position: absolute;
  top: 5.75px;
}
button {
  float: right;
  margin-top: 3.75px;
  margin-right: 5px;
}
</style>
