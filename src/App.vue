<template>
  <div class="max-w-[640px] mx-auto px-8">
    <h1 class="pt-10 pb-8 text-4xl font-bold text-neutral-600">Todo List</h1>
    <input
      type="text"
      class="w-full mb-4 p-3 px-4 border border-[#90a8ed] text-bold rounded"
      placeholder="검색어를 입력하세요."
      v-model="searchText"
    />
    <TodoForm @add-todo="addTodo" />
    <span class="block py-20 text-center" v-if="!filterTodos.length"
      >todo가 없습니다. 🙂</span
    >
    <TodoList
      :todos="filterTodos"
      @toggle-todo="toggleTodo"
      @delete-todo="deleteTodo"
    />
  </div>
</template>
<script>
import { ref, computed } from "vue";
import TodoForm from "./components/TodoForm";
import TodoList from "./components/TodoList";

export default {
  components: {
    TodoForm,
    TodoList,
  },
  setup() {
    const todos = ref([]);
    const searchText = ref("");

    const filterTodos = computed(() => {
      if (searchText.value) {
        return todos.value.filter((todo) => {
          return todo.subject.includes(searchText.value);
        });
      }

      return todos.value;
    });

    const addTodo = (todo) => {
      todos.value.push(todo);
    };

    const toggleTodo = (index) => {
      todos.value[index].completed = !todos.value[index].completed;
      console.log(todos.value[index].completed);
    };

    const deleteTodo = (todoId) => {
      todos.value.forEach((todo, index) => {
        if (todoId === todo.id) {
          todos.value.splice(index, 1);
        }
      });
    };

    return {
      todos,
      searchText,
      addTodo,
      toggleTodo,
      deleteTodo,
      filterTodos,
    };
  },
};
</script>
<style>
@import url("https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@100..900&display=swap");
</style>
computed,
