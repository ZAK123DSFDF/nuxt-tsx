<template>
  <counter-props
    :count="count"
    :message="message"
    @increment="handleIncrement"
    @reset="handleReset"
    @update:message="handleMessageUpdate"
  />
</template>

<script setup>
import CounterProps from "~/components/counterProps.vue";
import { ref } from "vue";

// Define refs to track values
const count = ref(5);
const message = ref("this is the passed");

// Define the increment handler
function handleIncrement(newValue) {
  count.value = newValue;
  console.log("Increment event received with value:", newValue);
}

// Define the reset handler
function handleReset() {
  count.value = 0;
  console.log("Reset event received");
}

// Define the message update handler
function handleMessageUpdate(newMessage) {
  message.value = newMessage;
  console.log("Message updated to:", newMessage);
}
</script>
