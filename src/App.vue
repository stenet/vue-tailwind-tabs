<script setup lang="ts">
import MyTab from './components/MyTab.vue';
import MyTabItem from './components/MyTabItem.vue';
</script>

<template>
  <div class="p-4">
    <MyTab>
      <MyTabItem text="Page 1">
        <div>Inhalt</div>
      </MyTabItem>
      <MyTabItem text="Page 2">
        <div>sdfasdfawef</div>
      </MyTabItem>
      <MyTabItem text="Page 3">
        <div>Idcvyxvxcvdfbnhalt</div>
      </MyTabItem>
      <MyTabItem text="Page 4">
        <div>Inhdbfbebea3alt</div>
      </MyTabItem>
    </MyTab>
  </div>
</template>
