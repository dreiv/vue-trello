<template>
  <div>
    {{ task.name }}
  </div>
</template>

<script>
import { mapGetters } from "vuex";

export default {
  props: {
    id: {
      type: Number,
      required: true,
    },
  },
  computed: {
    ...mapGetters(["getTask"]),
    task() {
      return this.getTask(this.id);
    },
  },
};
</script>

<style scoped>
</style>
