<template>
  <li class="item is-relative" @click="completed = !completed">
    <input type="checkbox" v-model="completed" />
    <span
      class="middle"
      v-if="!editting"
      @dblclick="editTodo()"
      :class="{completed: completed}"
    >{{title}}</span>
    <input
      v-else
      v-focus
      type="text"
      placeholder="Enter a task!"
      v-model="title"
      @blur="cancelEdit()"
      @keyup.enter="editedTodo()"
    />
    <div class="remove-item" @click="removeTodo(i)">&times;</div>
  </li>
</template>

<script>
export default {
  name: "TodoItem",
  props: {
    todo: {
      type: Object,
      required: true
    },
    index: Number
  },
  data() {
    return {
      id: this.todo.id,
      title: this.todo.title,
      completed: this.todo.completed,
      editing: this.todo.editing,
      beforceEditCache: ""
    };
  },
  methods: {
    removeTodo(index) {
      this.$emit("removedTodo", index)
    }
  }
};
</script>
