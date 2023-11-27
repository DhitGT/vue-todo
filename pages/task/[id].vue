<script setup>
import { onMounted } from "vue";
import { initFlowbite } from "flowbite";


onMounted(() => {
  initFlowbite();
});
const route = useRoute();
const router = useRouter();
const id = route.params.id;

import { createClient } from "@supabase/supabase-js";
const { baseUrl, apikey } = useAppConfig();
const supabase = createClient(baseUrl, apikey);
let data = null;
let categoryData = null;
let allData = null;

const user = useSupabaseUser();

if (user.value) {
  data = await supabase.from("list").select().eq("categoryid", id);
  categoryData = await supabase
    .from("category")
    .select()
    .eq("id", id)
    .eq("userid", user.value.id);
  if (!categoryData.data || categoryData.data.length === 0) {
    router.push("/"); 

    throw new Error("User not authorized to access this Tasks.");
  }
}

if (data) {
  allData = data.data;
}
console.log(categoryData);

async function updateSupabaseRow(id, value) {
  try {
    const { data, error } = await supabase
      .from("list")
      .update({ check: !value })
      .eq("id", id);

    if (error) {
      console.error("Error updating Supabase row:", error.message);
    } else {
      location.reload();

      console.log("Supabase row updated successfully:", data);
    }
  } catch (error) {
    console.error("Unexpected error:", error.message);
  }
}

async function deleteTask(id) {
  try {
    
    const { data, error } = await supabase
      .from("list")
      .delete()
      .eq("id", id);

    if (error) {
      console.error("Error updating Supabase row:", error.message);
    } else {
      location.reload();

      console.log("Supabase row updated successfully:", data);
    }
  } catch (error) {
    console.error("Unexpected error:", error.message);
  }
}
</script>
<template>
  <h1 align="center" class="text-4xl mb-24 font-bold">
    {{ categoryData.data[0].category }} Tasks
  </h1>
  <div class="relative overflow-x-auto shadow-md sm:rounded-lg">
    <table
      class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400"
    >
      <thead
        class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400"
      >
        <tr>
          <th scope="col" class="px-6 py-3">Task</th>
          <th scope="col" class="px-6 py-3">Check</th>
          <th scope="col" class="px-6 py-3">Action</th>
        </tr>
      </thead>
      <tbody v-for="(data, index) in allData" :key="index">
        <tr
          class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 hover:bg-gray-50 dark:hover:bg-gray-600"
        >
          <th
            scope="row"
            class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white"
            :class="{ 'line-through text-red-600': data.check }"
          >
            <p :class="{ 'text-gray-400': data.check }">
              {{ data.task }}
            </p>
          </th>
          <td class="px-6 py-4">
            <label
              class="relative inline-flex items-center mb-5 cursor-pointer"
            >
              <input
                type="checkbox"
                :checked="data.check"
                class="sr-only peer"
                @change="updateSupabaseRow(data.id, data.check)"
              />
              <div
                class="w-11 h-6 bg-gray-200 peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-blue-300 dark:peer-focus:ring-blue-800 rounded-full peer dark:bg-gray-700 peer-checked:after:translate-x-full rtl:peer-checked:after:-translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:start-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:w-5 after:h-5 after:transition-all dark:border-gray-600 peer-checked:bg-blue-600"
              ></div>
            </label>
          </td>

          <td class="px-6 py-4">
            <button
              @click="deleteTask(data.id)"
              class="dark:text-blue-500 hover:underline"
            >
              <svg
                class="w-6 h-6 text-gray-800 dark:text-white"
                aria-hidden="true"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 18 20"
              >
                <path
                  stroke="currentColor"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M1 5h16M7 8v8m4-8v8M7 1h4a1 1 0 0 1 1 1v3H6V2a1 1 0 0 1 1-1ZM3 5h12v13a1 1 0 0 1-1 1H4a1 1 0 0 1-1-1V5Z"
                />
              </svg>
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <div class="mt-16">
      <button
        class="relative inline-flex items-center justify-center w-96 p-0.5 mb-2 me-2 overflow-hidden text-sm font-medium text-gray-900 rounded-lg group bg-gradient-to-br from-green-400 to-blue-600 group-hover:from-green-400 group-hover:to-blue-600 hover:text-white dark:text-white focus:ring-4 focus:outline-none focus:ring-green-200 dark:focus:ring-green-800"
        data-modal-target="CreateTask-modal"
        data-modal-toggle="CreateTask-modal"
        type="button"
      >
        <span
          class="relative px-5 py-2.5 w-96 transition-all ease-in duration-75 bg-white dark:bg-gray-900 rounded-md group-hover:bg-opacity-0"
          >+ Create New Task</span
        >
      </button>
    </div>

    <FormsCreateTask :uid="user.id" :listid="id" />
  </div>
</template>
