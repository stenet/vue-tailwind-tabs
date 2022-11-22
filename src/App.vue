<script setup lang="ts">
import { ref } from 'vue';
import MyTab from './components/MyTab.vue';
import MyTabItem from './components/MyTabItem.vue';

const selectedIndex = ref(0);
</script>

<template>
  <div class="flex flex-col gap-4 p-4">
    <div>Selected tab {{ selectedIndex }}</div>
    <div>
      <button @click="() => (selectedIndex = 7)">select tab 7</button>
    </div>

    <MyTab v-model:selected-index="selectedIndex">
      <MyTabItem text="Page 1 🤭">
        <div>Content of Page 1 😉</div>
      </MyTabItem>
      <MyTabItem text="Page 2">
        <div>Content of Page 2 👏</div>
      </MyTabItem>
      <MyTabItem text="Page 3">
        <div>Content of Page 3 🚀</div>
      </MyTabItem>
      <MyTabItem text="Page 4">
        <div>Content of Page 4 🇪🇸</div>
      </MyTabItem>
      <MyTabItem text="Page 5">
        <div>Content of Page 5 🇨🇭</div>
      </MyTabItem>
      <MyTabItem text="Page 6">
        <div>Content of Page 6 😆</div>
      </MyTabItem>
      <MyTabItem text="Page 7">
        <div>Content of Page 7 😇</div>
      </MyTabItem>
      <MyTabItem text="Page 8">
        <div>Content of Page 8 😬</div>
      </MyTabItem>
      <MyTabItem text="Page 9">
        <div>Content of Page 9 🧐</div>
      </MyTabItem>
      <MyTabItem text="Page 10">
        <div>Content of Page 10 😝</div>
      </MyTabItem>
      <MyTabItem text="Page 11">
        <div>Content of Page 11 🫥</div>
      </MyTabItem>
      <MyTabItem text="Page 12">
        <div>Content of Page 12 🤬</div>
      </MyTabItem>
      <MyTabItem text="Page 13">
        <div>Content of Page 13 👹</div>
      </MyTabItem>
      <MyTabItem text="Page 14">
        <div>Content of Page 14 😈</div>
      </MyTabItem>
      <MyTabItem text="Page 15">
        <div>Content of Page 15 🤢</div>
      </MyTabItem>
    </MyTab>
  </div>
</template>
