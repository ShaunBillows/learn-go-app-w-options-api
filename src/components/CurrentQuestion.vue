<template>
  <p>{{ msg }}</p>
  <p>{{ question }}</p>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "CurrentQuestion",
  props: {
    msg: String,
    question: String,
  },
});
</script>
