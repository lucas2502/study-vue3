<template>
  <div>
    <h1>To do list! Old syntaxs</h1>
    <button @click="handleShowHideList">Show list</button>
    <br />
    <input type="text" @keyup.enter="addTask" v-focus v-model="currentTasks" />
    <ul v-if="showList">
      <li
        v-for="(task, index) in tasks"
        @dbclick="complete(task)"
        :key="`${task}-${index}`"
        class="task-item"
        :class="{ 'line-through': task.isDone }"
      >
        {{ task.name }}
        <button @click="remove(task)">&times;</button>
      </li>
    </ul>
  </div>
</template>

<script>
const focus = {
  inserted: (el) => {
    el.focus();
  },
};

export default {
  directives: {
    focus,
  },

  data: () => ({
    currentTasks: undefined,
    showList: false,
    tasks: [{ name: "Study vue3", isDone: false }],
  }),
  mounted() {
    console.log("Mounted now old syntaxs!");
  },
  methods: {
    handleShowHideList() {
      this.showList = !this.showList;
    },
    addTask() {
      this.tasks.push({ name: this.currentTasks, isDone: false });
      this.currentTasks = undefined;
    },
    complete(task) {
      this.tasks = this.tasks.map((i) => {
        if (i.name === task.name) {
          return { ...i, isDone: !i.isDone };
        }
      });
    },
    remove(task) {
      this.tasks = this.tasks.filter((i) => i.name !== task.name);
    },
  },
};
</script>

<style scoped>
.line-through {
  text-decoration: line-through;
}

.task-item {
  cursor: pointer;
}
</style>
