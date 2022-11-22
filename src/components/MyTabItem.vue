<script setup lang="ts">
const props = defineProps<{
  text: string;
}>();
</script>

<template>
  <div>
    <slot></slot>
  </div>
</template>
