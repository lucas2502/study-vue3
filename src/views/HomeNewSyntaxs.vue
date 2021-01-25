<template>
  <div>
    <h1>To do list! New syntaxs</h1>
    <button @click="handleShowHideList">Show list</button>
    <br />
    <input
      type="text"
      @keyup.enter="addTask"
      v-focus
      v-model="state.currentTasks"
    />
    <ul v-if="state.showList">
      <li
        v-for="(task, index) in state.tasks"
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
import { reactive } from "vue"; // import this for create values reactive
const focus = {
  inserted: (el) => {
    el.focus();
  },
};
export default {
  directives: {
    focus,
  },

  setup() {
    // not reactive
    const state = reactive({
      currentTasks: undefined,
      showList: false,
      tasks: [{ name: "Study vue3", isDone: false }],
    })

    function handleShowHideList() {
      state.showList = !state.showList;
    }
    function addTask() {
      state.tasks.push({ name: state.currentTasks, isDone: false });
      state.currentTasks = undefined;
    }
    function complete(task) {
      state.tasks = state.tasks.map((i) => {
        if (i.name === task.name) {
          return { ...i, isDone: !i.isDone };
        }
      });
    }
    function remove(task) {
      state.tasks = state.tasks.filter((i) => i.name !== task.name);
    }

    return {
      state,
      handleShowHideList,
      addTask,
      complete,
      remove,
    };
  },
};
</script>

<style scoped>
.line-through {
text-decoration: line-through;
}

.task-item {
cursor: pointer
}</style>
