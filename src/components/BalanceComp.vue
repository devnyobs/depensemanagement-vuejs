<script setup>
import { defineProps } from "vue";

const props = defineProps({
  total: {
    type: Number,
    required: true,
  },
});
</script>
<template>
  <h4>Votre Compte</h4>
  <h1 id="balance">$ {{ total }}</h1>
</template>
