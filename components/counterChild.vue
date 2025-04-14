<script setup>
const props = defineProps({
  count: Number,
});

const emit = defineEmits(["increment"]);

function increment() {
  emit("increment", props.count + 1);
}
</script>

<template>
  <div>
    <h3>Child Count View: {{ props.count }}</h3>
    <button @click="increment">Increment from Child</button>
  </div>
</template>
