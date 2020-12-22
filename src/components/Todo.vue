<template>
  <div
    class="todo"
    :class="todo.completed ? 'completed' : null"
    @click="todo.completed = !todo.completed"
  >
    <p class="text">{{ todo.description }}</p>
    <p class="remove" @click.stop="remove(todo.id)">X</p>
  </div>
</template>

<script>
export default {
  name: "todo",
  props: ["todo"],
  data() {
    return {
      completed: this.todo.completed
    };
  },
  methods: {
    remove(id) {
      this.$emit("remove", id);
      console.log("emit");
    }
  }
};
</script>

<style lang="sass" scoped>
.todo
  border-radius: 8px
  cursor: pointer
  background: #292929
  color: white
  font-weight: 700
  font-size: 20px
  display: flex
  align-items: center
  margin-bottom: 15px
  padding: 0 15px

.text
  display: inline
  word-break: break-word
  padding: 12px 0

.completed
  background: green
  > .text
    text-decoration: line-through

.remove
  color: red
  display: inline
  padding: 5px
  margin-left: 10px
  border-radius: 5px
  &:hover
    background-color: #DDDDDD33
</style>