<!--
Create a Val Town link in the presentation

Usage:

<Val id="id" height="80%" width="100%" />
-->
<script setup lang="ts">
defineProps<{
  id: string,
  width?: string,
  height?: string
}>()
</script>

<template>
  <iframe :src="`https://www.val.town/embed/${id}`" :width="width ?? '100%'" :height="height ?? '80%'" />
</template>