<template>
  <div
    class="block max-w-sm p-6 m-auto mt-36 bg-white border border-gray-200 rounded-lg shadow hover:bg-gray-100 dark:bg-gray-800 dark:border-gray-700 dark:hover:bg-gray-700"
  >
    <h1 align="center">Login</h1>
    <FormsLogin />
    <br />
    <hr />
    <br />
    <div
      class="text-sm flex font-medium text-gray-500 dark:text-gray-300 text-center"
    >
      Not registered?
      <button
        data-modal-target="Register-modal"
        data-modal-toggle="Register-modal"
        class="block w-max ms-1 text-white bfont-medium rounded-lg text-sm text-center hover:underline"
        type="button"
      >
        Register
      </button>
      <FormsRegister />
    </div>
  </div>
</template>
<script setup lang="ts">
definePageMeta({
  layout: "register",
});
import { onMounted } from "vue";
import { initFlowbite } from "flowbite";

// initialize components based on data attribute selectors
onMounted(() => {
  initFlowbite();
});
</script>
