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

    <div>
      <MyTab v-model:selected-index="selectedIndex">
        <MyTabItem text="Page 1 🤭"> Content of Page 1 😉 </MyTabItem>
        <MyTabItem text="Page 2"> Content of Page 2 👏 </MyTabItem>
        <MyTabItem text="Page 3"> Content of Page 3 🚀 </MyTabItem>
        <MyTabItem text="Page 4"> Content of Page 4 🇪🇸 </MyTabItem>
        <MyTabItem text="Page 5"> Content of Page 5 🇨🇭 </MyTabItem>
        <MyTabItem text="Page 6"> Content of Page 6 😆 </MyTabItem>
        <MyTabItem text="Page 7"> Content of Page 7 😇 </MyTabItem>
        <MyTabItem text="Page 8"> Content of Page 8 😬 </MyTabItem>
        <MyTabItem text="Page 9"> Content of Page 9 🧐 </MyTabItem>
        <MyTabItem text="Page 10"> Content of Page 10 😝 </MyTabItem>
        <MyTabItem text="Page 11"> Content of Page 11 🫥 </MyTabItem>
        <MyTabItem text="Page 12"> Content of Page 12 🤬 </MyTabItem>
        <MyTabItem text="Page 13"> Content of Page 13 👹 </MyTabItem>
        <MyTabItem text="Page 14"> Content of Page 14 😈 </MyTabItem>
        <MyTabItem text="Page 15"> Content of Page 15 🤢 </MyTabItem>
      </MyTab>
    </div>
  </div>
</template>
