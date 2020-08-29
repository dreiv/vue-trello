<template>
  <div class="backdrop" @click.self="close">
    <div class="dialog">
      {{ task.name }}
      <textarea class="description" :value="task.description" />
    </div>
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
  methods: {
    close() {
      this.$router.push({ name: "board" });
    },
  },
};
</script>

<style scoped>
.backdrop {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;

  background-color: #0005;
}

.dialog {
  background-color: white;
  width: 100%;
  max-width: 40rem;
  margin: 10% auto;
  padding: 1rem;
  border-radius: 0.25rem;
}

.description {
  box-sizing: border-box;
  display: block;
  width: 100%;
  margin-top: 0.5rem;
  padding: 0.5rem;
}
</style>
