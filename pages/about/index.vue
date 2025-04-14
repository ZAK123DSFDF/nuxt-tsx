<script setup>
import { ref } from "vue";
import CounterChild from "~/components/counterChild.vue";

const parentCount = ref(0);

function handleIncrement(newValue) {
  parentCount.value = newValue;
}
</script>

<template>
  <div>
    <h2>Parent Count: {{ parentCount }}</h2>
    <counter-child :count="parentCount" @increment="handleIncrement" />
  </div>
</template>
