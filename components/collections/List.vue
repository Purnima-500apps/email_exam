<template>
  <div
    class="grid grid-rows-3 grid-flow-col grid-cols-4 border w-[80vw] mx-auto my-5 rounded-lg pr-[4px]"
  >
    <div
      class="row-span-3 bg-gray-50 border-r overflow-hidden p-5 shadow rounded-l-lg"
    >
      <div class="pb-3 w-[100%]">
        <button
          type="submit"
          class="bg-white hover:bg-gray-50 hover:text-gray-800 border focus-visible:outline focus-visible:outline-2 focus-visible:outline-indigo-600 focus-visible:outline-offset-2 font-semibold inline-flex items-center justify-center px-3 py-3 rounded-md shadow-sm text-gray-600 text-sm w-[100%]"
        >
          <span>
            <IconCSS name="material-symbols:add" class="mr-2" size="20" />
          </span>
          Add Template
        </button>
      </div>
      <div
        v-for="(item, index) in getData.data._rawValue"
        :key="index"
        class="border p-4 rounded-md mb-3 shadow-sm bg-white"
      >
        <section @click="prefillData(item, index)">
          <h5 class="font-[500] text-md mb-2">{{ item.name }}</h5>
          <span class="text-gray-600">{{ item.subject }} - </span>
          <span class="text-gray-600">{{ item.body }}</span>
        </section>
        <section>
          <h5 class="font-[500] text-md mb-2">{{ item.name }}</h5>
          <span class="text-gray-600">{{ item.subject }} - </span>
          <span class="text-gray-600">{{ item.body }}</span>
          <div class="flex">
            <PencilSquareIcon
              @click="edit(item)"
              class="h-5 w-5"
              aria-hidden="true"
            ></PencilSquareIcon>
            <TrashIcon
              @click="deleteTemplate(item,index)"
              class="h-5 w-5"
              aria-hidden="true"
            ></TrashIcon>
          </div>
        </section>
      </div>
    </div>
    <CollectionsAdd @save="save" />
  </div>
</template>

<script setup lang="ts">
const firstname = ref({});
const indexValue = ref(0);

import { TrashIcon } from "@heroicons/vue/24/outline";
import {PencilSquareIcon} from "@heroicons/vue/24/outline"
//GET call
const getOptions = {
  method: "GET",
  headers: {
    "Content-Type": "application/json",
    Authorization: `Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1IjoiZjk0NjRlOTFjOTMxNGNkZGI5Zjg4ZWNkYWNhOWU5YjkiLCJkIjoiMTY4MDA4NiIsInIiOiJzYSIsInAiOiJmcmVlIiwiYSI6ImZpbmRlci5pbyIsImwiOiJ1czEiLCJleHAiOjE2ODMyNzk5ODF9._O3eNbQn0z2uDXKBxue7sMxToAQ_2io_TBw9lKXsFyk`,
  },
};
let getData = await useAuthLazyFetch(
  "https://v1-orm-lib.mars.hipso.cc/email-templates/?offset=0&limit=100&sort_column=id&sort_direction=desc",
  getOptions
);

//DELETE call

async function deleteTemplate(item, index) {
  console.log("index,item", index, item.uid);

  const deleteOptions = {
    headers: {
      "Content-Type": "application/json",
      Authorization: `Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1IjoiYzNlZTRlOWU1NTk2NDdjZDk4NGVmMzFhNjc4ODNkMmEiLCJkIjoiMTY4MDEwOCIsInIiOiJzYSIsInAiOiJmcmVlIiwiYSI6ImZpbmRlci5pbyIsImwiOiJ1czEiLCJleHAiOjE2ODMzNDkzOTB9.0V2YBlGbEMRMwE5sh1m-7WqAqfbB4_Fmqy9PX5rQoRo`,
    },
  };
  let getDeletedata = await useAuthLazyFetchDelete(
    `https://v1-orm-lib.mars.hipso.cc/email-templates/${item.uid}`,
    deleteOptions
  );
  console.log("getDeleteData", getDeletedata);
}
const save = (body: Object) => {
  console.log("dsss----->", body);
  getData.data._rawValue;
};

const prefillData = (item, index) => {};
const render=ref(0);
let id = ref("");
let name = ref("");
let body = ref("");
let subject = ref("");
const edit = (data: any) => {
  console.log("data", data);
  //$bus.$emit('edit-template', data)
  id.value = data.uid;
  name.value = data.name;
  body.value = data.body;
  subject.value = data.subject;
  render.value++;
};
</script>
