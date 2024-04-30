<template>
  <ul class="pt-10" v-if="todos.length">
    <li
      class="flex flex-row mb-2 p-2 px-4 bg-[#e9eefc] text-sm rounded-md"
      :class="{ 'bg-neutral-100': todo.completed }"
      v-for="todo in todos"
      :key="todo.id"
    >
      <label class="inline-flex items-center cursor-pointer">
        <input
          type="checkbox"
          :value="todo.completed"
          @change="toggleTodo(todo)"
          class="w-4 h-4 mr-2 border rounded-none"
        />
        <strong v-if="todo.completed" class="pr-2">완료🎉</strong>
        <span :class="{ 'text-neutral-400': todo.completed }">
          {{ todo.subject }}
        </span>
      </label>
      <button
        type="button"
        @click="deleteTodo(todo.id)"
        class="ml-auto py-2 px-3 bg-violet-500 text-white rounded hover:bg-violet-600 transition"
      >
        삭제
      </button>
    </li>
  </ul>
</template>
<script>
export default {
  props: {
    todos: {
      type: Array,
      required: true,
    },
  },
  emits: ["toggle-todo", "delete-todo"],
  setup(props, { emit }) {
    const toggleTodo = (todo) => {
      emit("toggle-todo", todo);
    };

    const deleteTodo = (todoId) => {
      emit("delete-todo", todoId);
    };

    return {
      toggleTodo,
      deleteTodo,
    };
  },
};
</script>
<style></style>
