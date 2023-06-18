<!--
Create a Wikipedia link in the presentation

Usage:

<Wiki id="id">Word name</Wiki>
-->
<script setup lang="ts">
defineProps<{
  id: string
}>()
</script>

<template>
  <sup><mdi-wikipedia /></sup>
  <a :href="`https://en.wikipedia.org/wiki/${id}`" target="_blank"><slot /></a>
</template>