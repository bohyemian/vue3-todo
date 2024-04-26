<template>
  <div>
    <form class="flex w-full" @submit.prevent="onSubmit">
      <input
        type="text"
        class="flex-1 p-3 px-4 border border-[#90a8ed] text-bold rounded"
        placeholder="todo를 작성해 주세요."
        v-model="todo"
        @blur="hideError"
      />
      <button
        class="w-[64px] ml-2 p-1 px-4 bg-[#90a8ed] text-white rounded hover:bg-[#6f8ce3] transition"
      >
        추가
      </button>
    </form>
    <span v-show="hasError" class="absolute py-2 text-xs text-red-600"
      >❌ todo를 입력해 주세요.</span
    >
    {{ todo }}
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  setup(props, context) {
    const todo = ref("");
    const hasError = ref(false);

    // TODO: input change event로 에러 메시지를 가리고 싶은데 blur 될 때 반영됨
    const hideError = () => {
      hasError.value = false;
    };

    const onSubmit = () => {
      if (todo.value === "") {
        hasError.value = true;
      } else {
        context.emit("add-todo", {
          id: Date.now(),
          subject: todo.value,
          completed: false,
        });

        todo.value = "";
        hasError.value = false;
      }
    };

    return {
      todo,
      hasError,
      hideError,
      onSubmit,
    };
  },
};
</script>

<style></style>
