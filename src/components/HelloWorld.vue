<script setup>
import { ref } from 'vue';

const count = ref(0);

</script>


<template>
  <h3>bonjour hubert</h3>
  <h3>count: {{ count }}</h3>
  <h3>{{ myMessage }}</h3>
  <button @click="count++" >increment</button>
</template>
