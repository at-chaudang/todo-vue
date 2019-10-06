<template>
  <div class="to-do-list">
    <h4>
      TodoList
      <span class="color-primary">CC</span>
    </h4>
    <div class="container">
      <input
        type="text"
        class="todo-input form-control"
        placeholder="Enter a task!"
        v-model="newTodo"
        @keyup.enter="addTodo"
      />
      <ul class="todos">
        <li v-for="(todo, i) in todosFilter" :key="i" class="item is-relative">
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
      <div class="section-2 footer">
        <button :class="{active: filter == 'all'}" @click="filter = 'all'">All</button>
        <button :class="{active: filter == 'active'}" @click="filter = 'active'">Active</button>
        <button :class="{active: filter == 'completed'}" @click="filter = 'completed'">Completed</button>
      </div>
    </div>
  </div>
</template>

<script>
import { log } from "util";
export default {
  name: "ToDoList",
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
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
.clr {
  &::after {
    display: block;
    content: "";
    clear: both;
  }
}
.left {
  float: left;
}
.right {
  float: right;
}
.middle {
  vertical-align: middle;
}
input {
  outline: 0;
}
.is-relative {
  position: relative;
}
.remove-item {
  position: absolute;
  top: 50%;
  right: 10px;
  transform: translateY(-50%);
}
.container {
  max-width: 500px;
  margin: auto;
}
ul {
  padding: 0;
  max-height: 300px;
  overflow: scroll;
}
li {
  list-style: none;
  text-align: left;
  padding: 5px 0 5px 10px;
  &:hover {
    cursor: pointer;
    background: #ececec;
  }
}
.color-primary {
  color: #42b983;
}
.form-control {
  line-height: 30px;
  box-shadow: 0 0 8px 0 rgba(0, 0, 0, 0.06);
  outline: 0;
  border: solid 1px #ececec;
  padding: 0 10px;
  width: 100%;
  border-radius: 4px;
}
.completed {
  text-decoration: line-through;
  color: gray;
}
.active {
  background: greenyellow;
}
button {
  outline: 0;
  height: 30px;
  padding: 0 20px;
  border-radius: 4px;
  cursor: pointer;
}
.footer {
  position: fixed;
  left: 50%;
  bottom: 10%;
  transform: translateX(-50%);
}
</style>
