<template>
  <AppDrop @drop="moveTaskOrColumn">
    <AppDrag
      class="column"
      :transferData="{
        type: 'column',
        fromColumnIndex: columnIndex
      }"
    >
      <div class="column-name">
        {{ column.name }}
      </div>
      <ColumnTask
        v-for="(task, $taskIndex) of column.tasks"
        :key="task.id"
        :task="task"
        :taskIndex="$taskIndex"
        :column="column"
        :columnIndex="columnIndex"
        :board="board"
      />

      <input
        type="text"
        class="add-item"
        placeholder="+ Enter new task"
        @keyup.enter="createTask($event, column.tasks)"
      />
    </AppDrag>
  </AppDrop>
</template>

<script>
import ColumnTask from "./ColumnTask";
import AppDrag from "./AppDrag";
import AppDrop from "./AppDrop";
import movingTasksAndColumnsMixin from "@/mixins/movingTasksAndColumnsMixin";

export default {
  components: { ColumnTask, AppDrag, AppDrop },
  mixins: [movingTasksAndColumnsMixin],
  methods: {
    createTask(event, tasks) {
      this.$store.commit("CREATE_TASK", {
        tasks,
        name: event.target.value,
      });

      event.target.value = "";
    },
    pickupColumn(event, fromColumnIndex) {
      event.dataTransfer.effectAllowed = "move";
      event.dataTransfer.dropEffect = "move";

      event.dataTransfer.setData("from-column-index", fromColumnIndex);
      event.dataTransfer.setData("type", "column");
    },
  },
};
</script>

<style scoped>
.column {
  background-color: lightgray;
  width: 20rem;
  flex-shrink: 0;
  padding: 0.5rem;
  border-radius: 0.25rem;
}

.column-name {
  margin-bottom: 1rem;
  font-weight: bold;
}

.add-item {
  border: none;
  height: 2.5rem;
  width: 100%;
  box-sizing: border-box;
}
.add-item:not(:focus) {
  background: none;
}
</style>
