<template>
  <div class="MyTop">
    <input type="text" class="top" @keydown.enter="add" />
    <!-- <button @click="unbind">点击解绑自定义事件</button> -->
  </div>
</template>

<script>
export default {
  name: "MyHeader",
  props: ["todos"],
  methods: {
    add(e) {
      this.$emit("addTodo", e.target.value);
      // this.addTodo(e.target.value);
      e.target.value = "";
    },
    unbind() {
      this.$off("addTodo");
    }
  }
};
</script>

<style scoped>
.top {
  border: 1px solid silver;
  text-indent: 5px;
  width: 500px;
  height: 40px;
}
</style>