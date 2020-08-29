<template>
  <div class="board">
    <div class="column" v-for="(column, index) of board.columns" :key="index">
      <div class="column-name">
        {{ column.name }}
      </div>
      <div class="task-list">
        <router-link
          class="task"
          :to="{ name: 'task', params: { id: task.id } }"
          v-for="task of column.tasks"
          :key="task.id"
        >
          <span>{{ task.name }}</span>
          <p v-if="task.description">{{ task.description }}</p>
        </router-link>
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
    createTask(event, tasks) {
      this.$store.commit("CREATE_TASK", {
        tasks,
        name: event.target.value,
      });

      event.target.value = "";
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
