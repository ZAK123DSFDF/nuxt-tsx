<script lang="tsx" setup>
import { ref, computed, toRef } from "vue";

// Props definition
const props = defineProps<{
  count?: { value: number };
  message?: string;
}>();

// Local state (fallback if props not provided)
const localCount = ref(0);
const localMessage = ref("Default message");

// Use props if available, otherwise use local state
const countRef = props.count ? toRef(props.count, "value") : localCount;
const messageRef = props.message ? ref(props.message) : localMessage;

// Child component that works with either parent-provided or local ref
const CounterChild = () => {
  const increment = () => {
    countRef.value++;
    console.log("After increment:", countRef.value);
  };

  return (
    <div class="child">
      <h3>{messageRef.value}</h3>
      <p>Current count: {countRef.value}</p>
      <button onClick={increment}>Increment from Child</button>
    </div>
  );
};

const StatusChild = () => {
  const reset = () => {
    countRef.value = 0;
    console.log("Resetting count");
  };

  return (
    <div class="child">
      <h3>Status Child</h3>
      <p>Status: {countRef.value > 5 ? "High" : "Low"}</p>
      <button onClick={reset}>Reset from Child</button>
    </div>
  );
};
</script>

<template>
  <div class="container">
    <h1>Counter Component</h1>

    <!-- Local controls (only shown when using local state) -->
    <div v-if="!$props.count" class="parent-controls">
      <button @click="props.count.value + 1">Increment from Parent</button>
      <input v-model="messageRef" placeholder="Type to update message" />
    </div>

    <!-- Child components -->
    <CounterChild />
    <StatusChild />

    <!-- Display current state -->
    <div class="state-display">
      <p>Current Count: {{ props.count }}</p>
      <p>Current Message: {{ props.message }}</p>
    </div>
  </div>
</template>

<style scoped>
.container {
  border: 1px solid #ddd;
  padding: 20px;
}
.parent-controls,
.child,
.state-display {
  margin: 15px 0;
  padding: 15px;
  border: 1px solid #eee;
}
button {
  margin: 5px;
  padding: 5px 10px;
}
</style>
