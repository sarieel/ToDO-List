<template>
  <div class="content">
    <myitem v-for="todo in todos" :key="todo.id" :todo="todo" />
  </div>
</template>

<script>
import myitem from "./MyItem";
export default {
  name: "MyList",
  props: ["todos"],
  components: { myitem },
};
</script>

<style scoped>
.content {
  position: relative;
  margin-top: 15px;
  border-bottom: 1px solid silver;
}
</style>
