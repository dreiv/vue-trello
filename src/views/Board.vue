<template>
  <div class="board">
    <div class="column" v-for="(column, index) of board.columns" :key="index">
      <div class="column-name">
        {{ column.name }}
      </div>
      <div class="task-list">
        <div class="task" v-for="task of column.tasks" :key="task.id">
          <h3>{{ task.name }}</h3>
          <p v-if="task.description">{{ task.description }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from "vuex";

export default {
  computed: {
    ...mapState(["board"]),
  },
};
</script>

<style scoped>
.board {
  display: flex;
}

.column {
  background-color: white;
}

.column-name {
  margin-bottom: 1rem;
  font-weight: bold;
}
</style>
