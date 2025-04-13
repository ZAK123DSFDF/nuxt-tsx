<script lang="tsx" setup>
import { ref } from "vue";

// Parent state
const parentCount = ref(0);
const parentMessage = ref("Parent message");
console.log("Is parentCount a ref?", isRef(parentCount));
// Child component with callback
const CounterChild = (props) => {
  const { count, message } = toRefs(props);
  const computedCount = computed({
    get: () => props.count,
    set: (val) => {
      console.log("Setting new value:", val);
      props.count = val;
    },
  });

  const increment = () => {
    computedCount.value++; // This triggers the setter
  };
  return (
    <div class="child">
      <h3>{message.value}</h3>
      <p>Current count: {computedCount.value}</p>
      <button onClick={increment}>Increment from Child</button>
    </div>
  );
};

// Another child with callback
const StatusChild = (props: {
  status: string;
  message: string;
  onReset: () => void;
}) => (
  <div class="child">
    <h3>{props.message}</h3>
    <p>Status: {props.status}</p>
    <button onClick={props.onReset}>Reset from Child</button>
  </div>
);
</script>

<template>
  <div class="container">
    <h1>Simplified Two-Way Communication</h1>

    <!-- Parent controls -->
    <div class="parent-controls">
      <button @click="parentCount++">Increment from Parent</button>
      <input v-model="parentMessage" placeholder="Type to update child" />
    </div>

    <!-- Child components with callbacks -->
    <counter-child
      :count="parentCount"
      :message="`Parent says: ${parentMessage}`"
    />

    <status-child
      :status="parentCount > 5 ? 'High' : 'Low'"
      message="Status Child"
    />

    <!-- Display current state -->
    <div class="state-display">
      <p>Parent Count: {{ parentCount }}</p>
      <p>Parent Message: {{ parentMessage }}</p>
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
