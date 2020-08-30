<template>
  <div class="board">
    <div
      class="column"
      v-for="(column, $columnIndex) of board.columns"
      :key="$columnIndex"
      @drop="moveTask($event, column.tasks)"
      @dragover.prevent
      @dragenter.prevent
    >
      <div class="column-name">
        {{ column.name }}
      </div>
      <div class="task-list">
        <div
          class="task"
          v-for="(task, $taskIndex) of column.tasks"
          :key="task.id"
          draggable
          @dragstart="pickupTask($event, $taskIndex, $columnIndex)"
          @click="goToTask(task)"
        >
          <span>{{ task.name }}</span>
          <p v-if="task.description">{{ task.description }}</p>
        </div>
      </div>

      <input
        type="text"
        class="add-task"
        placeholder="+ Enter new task"
        @keyup.enter="createTask($event, column.tasks)"
      />
    </div>

    <router-view />
  </div>
</template>

<script>
import { mapState } from "vuex";

export default {
  computed: {
    ...mapState(["board"]),
  },
  methods: {
    goToTask(task) {
      this.$router.push({ name: "task", params: { id: task.id } });
    },
    createTask(event, tasks) {
      this.$store.commit("CREATE_TASK", {
        tasks,
        name: event.target.value,
      });

      event.target.value = "";
    },
    pickupTask(event, taskIndex, fromColumnIndex) {
      console.log("touched");

      event.dataTransfer.effectAllowed = "move";
      event.dataTransfer.dropEffect = "move";

      event.dataTransfer.setData("task-index", taskIndex);
      event.dataTransfer.setData("from-column-index", fromColumnIndex);
    },
    moveTask(event, toTasks) {
      const fromColumnIndex = event.dataTransfer.getData("from-column-index");
      const fromTasks = this.board.columns[fromColumnIndex].tasks;
      const taskIndex = event.dataTransfer.getData("task-index");

      this.$store.commit("MOVE_TASK", {
        fromTasks,
        toTasks,
        taskIndex,
      });
    },
  },
};
</script>

<style scoped>
.board {
  display: flex;
  gap: 1rem;
}

.column {
  background-color: lightgray;
  width: 20rem;
  padding: 0.5rem;
  border-radius: 0.25rem;
}

.column-name {
  margin-bottom: 1rem;
  font-weight: bold;
}

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

.add-task {
  background: none;
  border: none;
  height: 2.5rem;
  width: 100%;
  box-sizing: border-box;
}
</style>
