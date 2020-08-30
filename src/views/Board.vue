<template>
  <div class="board">
    <BoardColumn
      v-for="(column, $columnIndex) of board.columns"
      :key="$columnIndex"
      :column="column"
      :columnIndex="$columnIndex"
      :board="board"/>

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
import BoardColumn from "@/components/BoardColumn";

export default {
  components: {
    BoardColumn,
  },
  computed: mapState(["board"]),
  methods: {
    createColumn(event) {
      this.$store.commit("CREATE_COLUMN", {
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
  align-items: flex-start;
  gap: 1rem;
}
</style>
