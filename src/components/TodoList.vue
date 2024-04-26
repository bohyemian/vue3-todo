<template>
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
          :value="todo.completed"
          @change="toggleTodo(index)"
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
</template>
<script>
export default {
  props: {
    todos: {
      type: Array,
      required: true,
    },
  },
  setup(props, context) {
    const toggleTodo = (index) => {
      context.emit("toggle-todo", index);
    };

    const deleteTodo = (index) => {
      context.emit("delete-todo", index);
    };

    return {
      toggleTodo,
      deleteTodo,
    };
  },
};
</script>
<style></style>
