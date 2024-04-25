<template>
  <form class="flex w-full" @submit.prevent="onSubmit">
    <input
      type="text"
      class="flex-1 p-3 px-4 border border-[#90a8ed] text-bold rounded"
      placeholder="write todo"
      v-model="todo"
    />
    <button
      class="w-[64px] ml-2 p-1 px-4 bg-[#90a8ed] text-white rounded hover:bg-[#6f8ce3] transition"
    >
      추가
    </button>
  </form>
</template>

<script>
export default {};
</script>

<style></style>
