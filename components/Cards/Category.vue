<script setup lang="ts">
const props = defineProps({
  data: {
    type: Object,
    default: {},
  },
});

import { createClient } from "@supabase/supabase-js";
const { baseUrl, apikey } = useAppConfig();
const supabase = createClient(baseUrl, apikey);
const user = useSupabaseUser();
let datalist = null;
let dataListDone = null;
if (user.value) {
  datalist = await supabase
    .from("list")
    .select()
    .eq("categoryid", props.data.id);
  dataListDone = await supabase
    .from("list")
    .select()
    .eq("categoryid", props.data.id)
    .eq("check", true);
}
console.log(datalist);
</script>

<template>
  <div
    class="max-w-2lg p-6 bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700"
  >
    <a href="#">
      <h5
        class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white"
      >
        {{ props.data.category }}
      </h5>
    </a>
    <p class="mb-3 font-normal flex text-gray-700 dark:text-gray-400">
      <span class="mr-3"
        ><svg
          class="w-6 h-6 text-gray-800 dark:text-white"
          aria-hidden="true"
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 17 10"
        >
          <path
            stroke="currentColor"
            stroke-linecap="round"
            stroke-width="2"
            d="M6 1h10M6 5h10M6 9h10M1.49 1h.01m-.01 4h.01m-.01 4h.01"
          /></svg></span
      >All Tasks : {{ datalist.data.length }}
    </p>
    <p class="mb-3 font-normal flex text-gray-700 dark:text-gray-400">
      <span class="mr-3">
        <svg
          class="w-6 h-6 text-gray-800 dark:text-white"
          aria-hidden="true"
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 20 20"
        >
          <path
            stroke="currentColor"
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="m7 10 2 2 4-4m6 2a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"
          />
        </svg>
      </span>
      Done : {{ dataListDone.data.length }}
    </p>
    <p class="mb-3 font-normal text-gray-700 flex dark:text-gray-400">
      <span class="mr-3">
        <svg
          class="w-6 h-6 text-gray-800 dark:text-white"
          aria-hidden="true"
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 21 18"
        >
          <path
            stroke="currentColor"
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M9.5 3h9.563M9.5 9h9.563M9.5 15h9.563M1.5 13a2 2 0 1 1 3.321 1.5L1.5 17h5m-5-15 2-1v6m-2 0h4"
          />
        </svg>
      </span>
      Remaining : {{ datalist.data.length - dataListDone.data.length }}
    </p>
    <NuxtLink
      :to="'/task/' + props.data.id"
      class="inline-flex items-center px-3 py-2 text-sm font-medium text-center text-white bg-blue-700 rounded-lg hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
    >
      <svg
        class="w-6 h-6 text-gray-800 dark:text-white"
        aria-hidden="true"
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 20 18"
      >
        <path
          stroke="currentColor"
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M10 16.5c0-1-8-2.7-9-2V1.8c1-1 9 .707 9 1.706M10 16.5V3.506M10 16.5c0-1 8-2.7 9-2V1.8c-1-1-9 .707-9 1.706"
        />
      </svg>
      <p class="ps-3">View All Tasks</p>
      <svg
        class="rtl:rotate-180 w-3.5 h-3.5 ms-2"
        aria-hidden="true"
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 14 10"
      >
        <path
          stroke="currentColor"
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M1 5h12m0 0L9 1m4 4L9 9"
        />
      </svg>
    </NuxtLink>
  </div>
</template>
