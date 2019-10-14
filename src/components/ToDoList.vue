<template>
  <div class="to-do-list">
    <div class="container">
      <input
        type="text"
        class="todo-input form-control"
        placeholder="Enter a task!"
        v-model="newTodo"
        @keyup.enter="addTodo"
      />
      <ul class="todos">
        <transition-group
          name="fade"
          enter-active-class="animated fadeInUp"
          leave-active-class="animated fadeOutDown"
        >
          <li
            v-for="(todo, i) in todosFilter"
            :key="todo.id"
            class="item is-relative"
            @click="todo.completed = !todo.completed"
          >
            <input type="checkbox" v-model="todo.completed" />
            <span
              class="middle"
              v-if="!todo.editting"
              @dblclick="editTodo(todo)"
              :class="{completed: todo.completed}"
            >{{todo.title}}</span>
            <input
              v-else
              v-focus
              type="text"
              placeholder="Enter a task!"
              v-model="todo.title"
              @blur="cancelEdit(todo)"
              @keyup.enter="editedTodo(todo)"
            />
            <div class="remove-item" @click="removeTodo(i)">&times;</div>
          </li>
        </transition-group>
      </ul>
      <hr />
      <div class="section-1 clr">
        <div class="left">
          <input type="checkbox" @click="checkAll()" :checked="!itemLeft && todos.length" />
          <span class="middle">Check all</span>
        </div>
        <div class="right">
          <span class="middle">{{itemLeft}} items left</span>
        </div>
      </div>
      <hr />
      <!-- <FilterTab :showClearCompletedBtnFromChild="showClearCompletedBtn" @onClearCompletedFromChild="onClearCompleted" /> -->
      <div class="section-2 footer left">
        <button :class="{active: filter == 'all'}" @click="filter = 'all'">All</button>
        <button :class="{active: filter == 'active'}" @click="filter = 'active'">Active</button>
        <button :class="{active: filter == 'completed'}" @click="filter = 'completed'">Completed</button>
      </div>
      <transition name="fade">
        <div class="right">
          <button v-if="showClearCompletedBtn" @click="onClearCompleted">Clear Completed</button>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import FilterTab from "./FilterTab";

export default {
  name: "ToDoList",
  components: {
    FilterTab
  },
  data() {
    return {
      newTodo: "",
      cachedTask: "",
      filter: "all",
      todos: [
        {
          id: 1,
          title: "Học NodeJS",
          completed: true,
          editting: false
        },
        {
          id: 2,
          title: "Học Angular 8",
          completed: false,
          editting: false
        },
        {
          id: 3,
          title: "Học VueJS",
          completed: false,
          editting: false
        }
      ]
    };
  },
  computed: {
    itemLeft() {
      return this.todos.filter(v => !v.completed).length;
    },
    todosFilter() {
      switch (this.filter) {
        case "active":
          return this.todos.filter(v => !v.completed);
          break;
        case "completed":
          return this.todos.filter(v => v.completed);
          break;
        default:
          return this.todos;
          break;
      }
    },
    showClearCompletedBtn() {
      return this.todos.filter(v => v.completed).length > 0;
    }
  },
  directives: {
    focus: {
      inserted: function(el) {
        el.focus();
      }
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim().length) {
        this.todos.push({
          id: this.idForTodo,
          title: this.newTodo,
          completed: false,
          editting: false
        });

        this.newTodo = "";
        this.idForTodo++;
      }
    },
    editTodo(todo) {
      this.cachedTask = todo.title;
      todo.editting = true;
    },
    editedTodo(todo) {
      todo.editting = false;
      if (!todo.title.trim().length) {
        todo.title = this.cachedTask;
      }
    },
    cancelEdit(todo) {
      todo.editting = false;
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    checkAll() {
      this.todos = this.todos.map(v => {
        v.completed = event.target.checked;
        return v;
      });
    },
    onClearCompleted() {
      console.log(1234);
      this.todos = this.todos.filter(v => !v.completed);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.2/animate.min.css");
.todo-input {
  width: 480px;
}
// CSS transitions
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
