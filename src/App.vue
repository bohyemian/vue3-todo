<template>
  <div class="max-w-[640px] mx-auto px-8">
    <h1 class="pt-10 pb-8 text-4xl font-bold text-neutral-600">Todo List</h1>
    <TodoForm @add-todo="addTodo" />
    <span class="block py-20 text-center" v-if="!todos.length"
      >등록 된 todo가 없습니다. 🙂</span
    >
    <TodoList
      :todos="todos"
      @toggle-todo="toggleTodo"
      @delete-todo="deleteTodo"
    />
  </div>
</template>
<script>
import { ref } from "vue";
import TodoForm from "./components/TodoForm";
import TodoList from "./components/TodoList";

export default {
  components: {
    TodoForm,
    TodoList,
  },
  setup() {
    const todos = ref([]);

    const addTodo = (todo) => {
      todos.value.push(todo);
    };

    const toggleTodo = (index) => {
      todos.value[index].completed = !todos.value[index].completed;
      console.log(todos.value[index].completed);
    };

    const deleteTodo = (index) => {
      todos.value.splice(index, 1);
    };

    return {
      todos,
      addTodo,
      toggleTodo,
      deleteTodo,
    };
  },
};
</script>
<style>
@import url("https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@100..900&display=swap");
</style>
computed,
