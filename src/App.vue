<template>
  <div class="max-w-[640px] mx-auto px-8">
    <h1 class="pt-10 pb-8 text-4xl font-bold text-neutral-500">📋Todo List</h1>
    <input
      type="text"
      class="w-full mb-4 p-3 px-4 border border-[#90a8ed] text-bold rounded"
      placeholder="검색어를 입력하세요."
      v-model="searchText"
    />
    <TodoForm @add-todo="addTodo" />
    <div
      :class="{ 'py-20 ': !error.length }"
      class="text-center"
      v-if="!filterTodos.length"
    >
      <span v-if="searchText.length">일치하는 </span>
      <span v-if="!error.length">todo가 없습니다. 🙂</span>
    </div>
    <div class="pt-1 text-red-600" v-if="error.length">{{ error }}🕳️</div>
    <TodoList
      :todos="filterTodos"
      @toggle-todo="toggleTodo"
      @delete-todo="deleteTodo"
    />
  </div>
</template>
<script>
import { ref, computed } from "vue";
import axios from "axios";
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
    const error = ref("");

    const getTodos = async () => {
      try {
        const res = await axios.get("https://tomboy.glitch.me/todos");
        todos.value = res.data;
      } catch (err) {
        error.value = err.message;
      }
    };

    getTodos();

    const filterTodos = computed(() => {
      if (searchText.value) {
        return todos.value.filter((todo) => {
          return todo.subject.includes(searchText.value);
        });
      }

      return todos.value;
    });

    const addTodo = async (todo) => {
      try {
        const res = await axios.post("https://tomboy.glitch.me/todos", {
          subject: todo.subject,
          completed: todo.completed,
        });

        todos.value.push(res.data);
        error.value = "";
      } catch (err) {
        error.value = err.message;
      }
    };

    const toggleTodo = async (dbTodo) => {
      try {
        await axios.patch(`https://tomboy.glitch.me/todos/${dbTodo.id}`, {
          completed: !dbTodo.completed,
        });

        todos.value.forEach((todo, index) => {
          if (todo.id === dbTodo.id) {
            todos.value[index].completed = !todos.value[index].completed;
          }
        });
      } catch (err) {
        error.value = err.message;
      }
    };

    const deleteTodo = async (todoId) => {
      try {
        await axios.delete(`https://tomboy.glitch.me/todos/${todoId}`);

        todos.value.forEach((todo, index) => {
          if (todoId === todo.id) {
            todos.value.splice(index, 1);
          }
        });

        error.value = "";
      } catch (err) {
        error.value = err.message;
      }
    };

    return {
      todos,
      searchText,
      addTodo,
      toggleTodo,
      deleteTodo,
      filterTodos,
      error,
    };
  },
};
</script>
<style>
@import url("https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@100..900&display=swap");
</style>
computed,
