<!--
Create a NPM package link in the presentation

Usage:

<NPM id="id">NPM package name</NPM>
-->
<script setup lang="ts">
defineProps<{
  id: string
}>()
</script>

<template>
  <sup><mdi-npm /></sup>
  <a :href="`https://www.npmjs.com/package/${id}`" target="_blank"><slot /></a>
</template>