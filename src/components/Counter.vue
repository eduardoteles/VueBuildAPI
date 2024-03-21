<script setup>
    import { ref } from 'vue'
    const count = ref(0)
</script>

<template>
    <button @click="count++">Clicou aqui {{ count }} vezes</button>
 </template>