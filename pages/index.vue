<script lang="ts" setup>
import { onMounted } from "vue";
import { initFlowbite } from "flowbite";

// initialize components based on data attribute selectors
onMounted(() => {
  initFlowbite();
});
import { createClient } from "@supabase/supabase-js";
const { baseUrl, apikey } = useAppConfig();
const supabase = createClient(baseUrl, apikey);
let data = null;
let allData = null;

const user = useSupabaseUser();

if (user.value) {
  data = await supabase.from("category").select().eq("userid", user.value.id);
}

if (data) {
  allData = data.data;
}

function getTaskLen() {}
</script>

<template>
  <h4
    class="mb-4 text-xl font-extrabold text-gray-900 dark:text-white md:text-xl lg:text-xl"
  >
    <span
      class="text-transparent bg-clip-text bg-gradient-to-r to-emerald-600 from-sky-400"
      >WELCOME</span
    >
    {{ user.email }}
  </h4>
  <p class="text-lg font-normal text-gray-500 lg:text-xl dark:text-gray-400">
    At Here Are Your Lists
  </p>

  <h1 align="" class="text-3xl"></h1>
  <div>
    <template v-for="(data, index) in allData" :key="index">
      <div class="flex justify-evenly flex-col">
        <CardsCategory class="mb-5" :data="data" />
      </div>
    </template>

    <div class="mt-16">
      <button
        class="relative inline-flex items-center justify-center w-96 p-0.5 mb-2 me-2 overflow-hidden text-sm font-medium text-gray-900 rounded-lg group bg-gradient-to-br from-green-400 to-blue-600 group-hover:from-green-400 group-hover:to-blue-600 hover:text-white dark:text-white focus:ring-4 focus:outline-none focus:ring-green-200 dark:focus:ring-green-800"
        data-modal-target="CreateCategory-modal"
        data-modal-toggle="CreateCategory-modal"
        type="button"
      >
        <span
          class="relative px-5 py-2.5 w-96 transition-all ease-in duration-75 bg-white dark:bg-gray-900 rounded-md group-hover:bg-opacity-0"
          >Create New List</span
        >
      </button>
    </div>

    <FormsCreateCategory :uid="user.id" />
  </div>
</template>
