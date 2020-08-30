<template>
  <AppDrop
    @drop="moveTaskOrColumn"
  >
    <AppDrag
      class="task"
      :transferData="{
        type: 'task',
        fromColumnIndex: columnIndex,
        fromTaskIndex: taskIndex
      }"
      @click="goToTask(task)"
    >
      <span>{{ task.name }}</span>
      <p v-if="task.description">{{ task.description }}</p>
    </AppDrag>
  </AppDrop>
</template>

<script>
import movingTasksAndColumnsMixin from "@/mixins/movingTasksAndColumnsMixin";
import AppDrag from "./AppDrag";
import AppDrop from "./AppDrop";

export default {
  components: { AppDrag, AppDrop },
  mixins: [movingTasksAndColumnsMixin],
  props: {
    task: {
      type: Object,
      required: true,
    },
    taskIndex: {
      type: Number,
      required: true,
    },
  },
  methods: {
    goToTask(task) {
      this.$router.push({ name: "task", params: { id: task.id } });
    },
  },
};
</script>

<style scoped>
.task {
  display: block;
  color: inherit;
  background-color: white;
  margin-bottom: 0.5rem;
  padding: 0.5rem;
  border-radius: 0.25rem;
  font-weight: bold;
}
.task:not(:hover) {
  text-decoration: none;
}
</style>
