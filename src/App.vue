<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput v-on:addTodo="addTodo"></TodoInput>
    <TodoList v-bind:propsdata="todoItems" @removeTodo="removeTodo"></TodoList>
    <TodoFooter v-on:removeAll="clearAll"></TodoFooter>
  </div>
</template>

<script>
import TodoFooterVue from "./components/TodoFooter.vue";
import TodoListVue from "./components/TodoList.vue";
import TodoInputVue from "./components/TodoInput.vue";
import TodoHeaderVue from "./components/TodoHeader.vue";

export default {
  data() {
    return {
      todoItems: []
    };
  },
  created() {
    if (localStorage.length > 0) {
      for (var i = 0; i < localStorage.length; i++) {
        this.todoItems.push(localStorage.key(i));
      }
    }
  },
  methods: {
    addTodo(todoItem) {
      localStorage.setItem(todoItem, todoItem);
      this.todoItems.push(todoItem);
    },
    clearAll() {
      localStorage.clear();
      this.todoItems = [];
    },
    removeTodo(todoItem, index) {
      localStorage.removeItem(todoItem);
      this.todoItems.splice(index, 1);
    }
  },
  components: {
    TodoHeader: TodoHeaderVue,
    TodoInput: TodoInputVue,
    TodoList: TodoListVue,
    TodoFooter: TodoFooterVue
  }
};
</script>

<style>
body {
  text-align: center;
  background-color: bisque;
}
input {
  border-style: groove;
  width: 200px;
}
button {
  border-style: groove;
}
.shadow {
  box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03);
}
</style>

