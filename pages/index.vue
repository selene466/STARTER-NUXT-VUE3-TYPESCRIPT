<script setup lang="ts">
import { themeChange } from "theme-change";

// layouts
import DefaultLayout from "~/layouts/DefaultLayout.vue";

// components
import ButtonClick from "~/components/ButtonClick.vue";

// mounted
onMounted(() => {
  themeChange(false);
});

// ref
const showToast = ref(false);
const showToastResultBackend = ref(false);
const responseResultBackend = ref("");

// function
const btnClick = () => {
  console.log("Clicked");
  showToast.value = true;
  setTimeout(() => (showToast.value = false), 3000);
};

const btnTestBackend = async () => {
  const { data, error } = await useFetch("/api/test");

  if (error.value) {
    console.error(error.value);
  } else if (data.value) {
    console.log(data.value);
    showToastResultBackend.value = true;
    responseResultBackend.value = data.value.message;
    showToastResultBackend.value = true;
    setTimeout(() => {
      showToastResultBackend.value = false;
      responseResultBackend.value = "";
    }, 3000);
  }
  // useFetch /api/test
};
</script>
<template>
  <DefaultLayout>
    <div
      class="container mx-auto flex h-[90%] max-w-screen-lg flex-col items-center justify-center border border-base-300 bg-base-100 p-5 shadow-xl"
    >
      <h1 class="mb-4 text-4xl font-bold">Welcome to Starter Template</h1>
      <div class="mb-8 text-sm tracking-wide">
        <ul class="list-inside list-disc">
          <li>Vite + Vue3 + Tailwind v3</li>
          <li>DaisyUI v4 + theme changer</li>
          <li>Iconify: mdi</li>
          <li>Typescript</li>
        </ul>
      </div>
      <div class="grid grid-cols-2 place-content-center gap-4">
        <ButtonClick @click="btnClick()">
          Click Toast
          <Icon name="mdi:cursor-default-click" class="inline align-middle" />
        </ButtonClick>
        <ButtonClick @click="btnTestBackend()">
          Test Backend
          <Icon name="mdi:star-box-multiple" class="inline align-middle" />
        </ButtonClick>
        <button
          class="btn btn-neutral text-neutral-content"
          data-toggle-theme="dark,light"
        >
          Change Theme
          <Icon name="mdi:theme-light-dark" class="inline align-middle" />
        </button>
      </div>
      <div v-if="showToast" class="toast toast-end">
        <div class="alert alert-success shadow-xl">
          <span>This is toast</span>
        </div>
      </div>
      <div
        v-if="showToastResultBackend && responseResultBackend"
        class="toast toast-end"
      >
        <div class="alert alert-success shadow-xl">
          <span>{{ responseResultBackend }}</span>
        </div>
      </div>
    </div>
  </DefaultLayout>
</template>
