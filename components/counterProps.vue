<script lang="tsx" setup>
import { computed } from "vue";

const props = defineProps({
  count: Number,
  message: String,
});
const emit = defineEmits(["increment", "reset", "update:message"]);
const parentOnlyVar = ref(0);
// Computed property for two-way binding
const parentOnlyUp = () => {
  parentOnlyVar.value = parentOnlyVar.value + 1;
};
function handleMessageChange(e) {
  emit("update:message", e.target.value);
}
function increment() {
  emit("increment", props.count + 1);
}

function reset() {
  emit("reset");
}

// Child component with callback
const CounterChild = () => {
  console.log("count rendered");
  return (
    <div class="child">
      <h3>{props.message}</h3>
      <p>Current count: {props.count}</p>
      <button onClick={increment}>Increment from Child</button>
    </div>
  );
};

// Refactored StatusChild component
const StatusChild = () => {
  const status = props.count > 5 ? "High" : "Low";
  console.log("status rendered");
  return (
    <>
      <div class="child">
        <h3>Status Child</h3>
        <p>Status: {status}</p>
        <button onClick={reset}>Reset from Child</button>
      </div>
      <div>this is the check</div>
    </>
  );
};
</script>

<template>
  <div class="container">
    <h1>Simplified Two-Way Communication</h1>
    <p>this is parentOnly var: {{ parentOnlyVar }}</p>
    <button @click="parentOnlyUp">parentOnlyVarUp</button>
    <!-- Parent controls -->
    <div class="parent-controls">
      <button @click="increment">Increment from Parent</button>
      <input
        v-model="props.message"
        @input="handleMessageChange"
        @change="() => console.log('input changed')"
        placeholder="Type to update child"
      />
    </div>

    <!-- Child components with callbacks -->
    <counter-child />

    <status-child />

    <!-- Display current state -->
    <div class="state-display">
      <p>Parent Count: {{ props.count }}</p>
      <p>Parent Message: {{ props.message }}</p>
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
