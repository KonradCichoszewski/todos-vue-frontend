<template>
  <div class="todo">
    <div
      class="plate"
      :class="todo.completed ? 'completed' : null"
      @click="update(todo)"
    >
      <p class="text">{{ todo.text }}</p>
      <img
        src="../assets/remove.png"
        class="remove"
        @click.stop="remove(todo.id)"
      />
    </div>
    <div class="edit"></div>
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
    },
    update(todo) {
      this.$emit("update", todo);
    }
  }
};
</script>

<style lang="sass" scoped>
.plate
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
  background: #8CFFC0
  > .text
    text-decoration: line-through
    color: #292929

.remove
  height: 27px
  color: red
  display: inline
  padding: 5px
  margin-left: 10px
  border-radius: 5px
  &:hover
    background-color: #DDDDDD33
</style>