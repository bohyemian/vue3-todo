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
      v-if="!todos.length"
    >
      <span v-if="searchText.length">일치하는 </span>
      <span v-if="!error.length">todo가 없습니다. 🙂</span>
    </div>
    <div class="pt-1 text-red-600" v-if="error.length">{{ error }}🕳️</div>
    <TodoList
      :todos="todos"
      @toggle-todo="toggleTodo"
      @delete-todo="deleteTodo"
    />
    <div class="flex justify-center pt-3" v-if="numberOfPages > 0">
      <button
        v-if="currentPage !== 1"
        class="mr-2 px-2 bg-neutral-300 text-xs rounded"
        @click="getTodos(currentPage - 1)"
      >
        prev
      </button>
      <!-- TODO: 조건 한개에 여러개의 클래스 바인딩 -->
      <button
        type="button"
        @click="getTodos(page)"
        class="items-center justify-center w-7 h-7 bg-neutral-100 text-xs rounded"
        :class="{
          'ml-1': page > 1,
          'bg-blue-200': currentPage === page,
          'text-white': currentPage === page,
        }"
        v-for="page in numberOfPages"
        :key="page"
      >
        {{ page }}
      </button>
      <button
        type="button"
        v-if="currentPage !== numberOfPages"
        class="ml-2 px-2 bg-neutral-300 text-xs rounded"
        @click="getTodos(currentPage + 1)"
      >
        next
      </button>
    </div>
  </div>
</template>
<script>
import { ref, computed, watch } from "vue";
import axios from "axios";
import TodoForm from "./components/TodoForm";
import TodoList from "./components/TodoList";

export default {
  components: {
    TodoForm,
    TodoList,
  },
  setup() {
    const serverUrl = "https://tomboy.glitch.me/todos";
    // const serverUrl = "http://localhost:3000/todos";
    const todos = ref([]);
    const error = ref("");
    const searchText = ref("");
    const currentPage = ref(1);
    const limit = 5;
    const numberOfTodos = ref(0);
    const numberOfPages = computed(() => {
      return Math.ceil(numberOfTodos.value / limit);
    });

    const getTodos = async (page = currentPage.value) => {
      currentPage.value = page;

      try {
        const res = await axios.get(
          `${serverUrl}/?subject_like=${searchText.value}&_sort=id&_order=desc&_page=${page}&_limit=${limit}`
        );
        todos.value = res.data;
        numberOfTodos.value = res.headers["x-total-count"];
      } catch (err) {
        error.value = err.message;
      }
    };

    getTodos();

    let timeout;
    watch(searchText, () => {
      clearTimeout(timeout);

      timeout = setTimeout(() => {
        getTodos(1);
      }, 1000);
    });

    //todo는 자식 컴포넌트에서 전달한 값
    const addTodo = async (todo) => {
      try {
        await axios.post(serverUrl, {
          subject: todo.subject,
          completed: todo.completed,
        });

        getTodos(1);
        error.value = "";
      } catch (err) {
        error.value = err.message;
      }
    };

    const toggleTodo = async (dbTodo) => {
      try {
        await axios.patch(`${serverUrl}/${dbTodo.id}`, {
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
        await axios.delete(`${serverUrl}/${todoId}`);

        getTodos(1);
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
      error,
      getTodos,
      numberOfPages,
      currentPage,
    };
  },
};
</script>
<style>
@import url("https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@100..900&display=swap");
</style>
computed,
