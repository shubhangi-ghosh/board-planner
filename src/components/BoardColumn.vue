<template>
  <div
    class="column"
    draggable
    @dragstart.self="pickUpColumn($event, columnIndex, 'column')"
    @drop="moveTaskOrColumn($event, column.tasks, columnIndex)"
    @dragover.prevent
    @dragenter.prevent
  >
    <div class="text-center mb-2 font-bold text-lg capitalize">
      <h3>{{ column.name }}</h3>
    </div>
    <div class="list-reset">
      <ColumnTask
        v-for="(task, $taskIndex) of column.tasks"
        :key="$taskIndex"
        :task="task"
        :taskIndex="$taskIndex"
        :columnIndex="columnIndex"
        :column="column"
        :board="board"
      />
      <input
        type="text"
        class="block w-full bg-transparent p-2"
        placeholder="+ Enter task name"
        @keyup.enter="createNewTask($event, column.tasks)"
      />
    </div>
  </div>
</template>

<script>
import ColumnTask from "@/components/ColumnTask.vue";
import moveTasksAndColumnMixin from "../mixins/moveTasksAndColumnMixin.js";
export default {
  name: "BoardColumn",
  mixins: [moveTasksAndColumnMixin],
  components: {
    ColumnTask
  },
  methods: {
    createNewTask(e, tasks) {
      this.$store.dispatch("createTask", { e, tasks });
      e.target.value = "";
    },
    pickUpColumn(event, fromColumnIndex, type) {
      event.dataTransfer.effectAllowed = "move";
      event.dataTransfer.dropEffect = "move";

      event.dataTransfer.setData("column-index", fromColumnIndex);
      event.dataTransfer.setData("type", type);
    }
  }
};
</script>

<style lang="scss">
.column {
  @apply rounded shadow text-left bg-gray-500 p-2 mr-4;
  min-width: 350px;
  max-width: 400px;
  // max-height: 500px;
}
.list-reset input:focus {
  //   outline: none;
  @apply rounded;
  box-shadow: 2px 2px 5px 2px rgba(255, 255, 255, 0.5);
}
</style>
