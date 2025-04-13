<script lang="tsx" setup>
import { ref, computed, isRef } from "vue";

// Parent state
const parentCount = ref(0);
const parentMessage = ref("Parent message");

// Child component with proper value access
const CounterChild = (props: {
  count: { value: number };
  message: string;
  onIncrement: (newValue: number) => void;
}) => {
  // Create writable computed property
  const refCount = toRef(props.count, "value");
  const localCount = computed({
    get: () => {
      console.log("Getting count:", refCount);
      return refCount;
    },
    set: (val) => {
      console.log("Setting count to:", val);
      props.onIncrement(val);
    },
  });

  // Proper way to log computed ref value
  console.log("Current count value:", localCount.value);
  console.log("Is localCount reactive?", isRef(localCount));

  return (
    <div class="child">
      <h3>{props.message}</h3>
      <p>Current count: {localCount.value.value}</p>
      <button
        onClick={() => {
          localCount.value++;
          console.log("After increment:", localCount.value);
        }}
      >
        Increment from Child
      </button>
    </div>
  );
};

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
    <h1>Final Working Solution</h1>

    <!-- Parent controls -->
    <div class="parent-controls">
      <button @click="parentCount++">Increment from Parent</button>
      <input v-model="parentMessage" placeholder="Type to update child" />
    </div>

    <!-- Child components -->
    <counter-child
      :count="parentCount"
      :message="`Parent says: ${parentMessage}`"
      :on-increment="
        (newValue) => {
          console.log('Parent updating count to:', newValue);
          parentCount.value = newValue;
        }
      "
    />

    <status-child
      :status="parentCount > 5 ? 'High' : 'Low'"
      message="Status Child"
      :on-reset="
        () => {
          console.log('Parent resetting count');
          parentCount.value = 0;
        }
      "
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
