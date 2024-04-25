<template>
  <div class="max-w-[640px] mx-auto px-8">
    <h1 class="pt-10 pb-8 text-4xl font-bold text-[#23a094]">Todo List</h1>
    <TodoForm />
    <span v-show="hasError" class="absolute py-2 text-xs text-red-600"
      >❌ todo를 입력해 주세요.</span
    >
    <span class="block py-20 text-center" v-if="!todos.length"
      >추가된 todo가 없습니다. 🙂</span
    >
    <ul class="pt-10" v-if="todos.length">
      <li
        class="flex flex-row mb-2 p-2 px-4 bg-[#e9eefc] text-sm rounded-md"
        v-for="(todo, index) in todos"
        :key="todo.id"
      >
        <label
          class="inline-flex items-center cursor-pointer"
          :class="{ 'line-through': todo.completed }"
        >
          <input
            type="checkbox"
            v-model="todo.completed"
            @change="completed"
            class="w-4 h-4 mr-2 border rounded-none"
          />
          {{ todo.subject }}
        </label>
        <button
          type="button"
          @click="deleteTodo(index)"
          class="ml-auto py-2 px-3 bg-violet-500 text-white rounded hover:bg-violet-600 transition"
        >
          삭제
        </button>
      </li>
    </ul>
  </div>
</template>
<script>
import { ref } from "vue";
import TodoForm from "./components/TodoForm";

export default {
  components: {
    TodoForm,
  },
  setup() {
    const hasError = ref(false);
    const todo = ref("");
    const todos = ref([]);

    const onSubmit = () => {
      if (todo.value === "") {
        hasError.value = true;
      } else {
        todos.value.push({
          id: Date.now(),
          subject: todo.value,
          completed: false,
        });

        todo.value = "";
        hasError.value = false;
      }
    };

    const deleteTodo = (index) => {
      todos.value.splice(index, 1);
    };

    return {
      hasError,
      todo,
      todos,
      onSubmit,
      deleteTodo,
    };
  },
};
</script>
<style>
@import url("https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@100..900&display=swap");
</style>
computed,
