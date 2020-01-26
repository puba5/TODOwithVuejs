<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput v-on:addTodo="addTodo"></TodoInput>
    <TodoList v-bind:propsdata="todoItems" @editTodo="editTodo" @removeTodo="removeTodo"></TodoList>
    <TodoFooter v-on:removeAll="clearAll"></TodoFooter>
    <modal v-if="editModal" @close="editModal = false">
      <h3 slot="header">{{todoItem}}</h3>
      <input
        slot="body"
        type="text"
        v-model="editItem"
        v-on:keyup.enter="editContent"
        placeholder="수정할 계획을 입력하세요"
      >
      <span slot="footer" @click="editModal = false; editContent()">
        <i class="closeModalBtn far fa-check-square" aria-hidden="true"></i>
      </span>
    </modal>
  </div>
</template>

<script>
import Modal from "./components/common/Modal.vue";
import TodoFooterVue from "./components/TodoFooter.vue";
import TodoListVue from "./components/TodoList.vue";
import TodoInputVue from "./components/TodoInput.vue";
import TodoHeaderVue from "./components/TodoHeader.vue";

export default {
  data() {
    return {
      todoItems: [],
      editModal: false,
      editItem: "",
      todoItem: "",
      editIndex: 0
    };
  },
  created() {
    if (localStorage.length > 0) {
      for (var i = 0; i < localStorage.length; i++) {
        if (localStorage.key(i) != "loglevel:webpack-dev-server")
          this.todoItems.push(localStorage.key(i));
      }
    }
  },
  methods: {
    addTodo(todoItem) {
      if (this.todoItems.find(element => element == todoItem) != todoItem) {
        localStorage.setItem(todoItem, todoItem);
        this.todoItems.push(todoItem);
      }
    },
    clearAll() {
      localStorage.clear();
      this.todoItems = [];
    },
    removeTodo(todoItem, index) {
      localStorage.removeItem(todoItem);
      this.todoItems.splice(index, 1);
    },
    editTodo(todoItem, index) {
      this.editModal = !this.editModal;
      this.editIndex = index;
      this.todoItem = todoItem;
      //editItem = this.todoItem;
      // this.editModal = !this.editModal;
      console.log("Edit");
    },
    editContent() {
      this.addTodo(this.editItem);
      this.removeTodo(this.todoItem, this.editIndex);
      this.editItem = "";
    }
  },
  components: {
    TodoHeader: TodoHeaderVue,
    TodoInput: TodoInputVue,
    TodoList: TodoListVue,
    TodoFooter: TodoFooterVue,
    Modal: Modal
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

