<template>
  <div class="board">
    <div
      class="column"
      v-for="(column, $columnIndex) of board.columns"
      :key="$columnIndex"
      draggable
      @drop="moveTaskOrColumn($event, column.tasks, $columnIndex)"
      @dragover.prevent
      @dragenter.prevent
      @dragstart.self="pickupColumn($event, $columnIndex)"
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
          @dragover.prevent
          @dragenter.prevent
          @drop.stop="moveTaskOrColumn($event, column.tasks, $columnIndex, $taskIndex)"
        >
          <span>{{ task.name }}</span>
          <p v-if="task.description">{{ task.description }}</p>
        </div>
      </div>

      <input
        type="text"
        class="add-item"
        placeholder="+ Enter new task"
        @keyup.enter="createTask($event, column.tasks)"
      />
    </div>

    <div class="column">
       <input
        type="text"
        class="add-item"
        placeholder="+ Enter new column"
        @keyup.enter="createColumn($event)"
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
    createColumn(event) {
      this.$store.commit("CREATE_COLUMN", {
        name: event.target.value,
      });

      event.target.value = "";
    },
    pickupTask(event, taskIndex, fromColumnIndex) {
      event.dataTransfer.effectAllowed = "move";
      event.dataTransfer.dropEffect = "move";

      event.dataTransfer.setData("from-task-index", taskIndex);
      event.dataTransfer.setData("from-column-index", fromColumnIndex);
      event.dataTransfer.setData("type", "task");
    },
    pickupColumn(event, fromColumnIndex) {
      event.dataTransfer.effectAllowed = "move";
      event.dataTransfer.dropEffect = "move";

      event.dataTransfer.setData("from-column-index", fromColumnIndex);
      event.dataTransfer.setData("type", "column");
    },
    moveTaskOrColumn(event, toTasks, toColumnIndex, toTaskIndex) {
      const type = event.dataTransfer.getData("type");

      if (type === "task") {
        this.moveTask(
          event,
          toTasks,
          toTaskIndex !== undefined ? toTaskIndex : toTasks.length
        );
      } else {
        this.moveColumn(event, toColumnIndex);
      }
    },
    moveTask(event, toTasks, toTaskIndex) {
      const fromColumnIndex = event.dataTransfer.getData("from-column-index");
      const fromTasks = this.board.columns[fromColumnIndex].tasks;
      const fromTaskIndex = event.dataTransfer.getData("from-task-index");

      this.$store.commit("MOVE_TASK", {
        fromTasks,
        fromTaskIndex,
        toTasks,
        toTaskIndex,
      });
    },
    moveColumn(event, toColumnIndex) {
      const fromColumnIndex = event.dataTransfer.getData("from-column-index");

      this.$store.commit("MOVE_COLUMN", {
        fromColumnIndex,
        toColumnIndex,
      });
    },
  },
};
</script>

<style scoped>
.board {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
}

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
