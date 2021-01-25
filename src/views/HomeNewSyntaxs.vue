<template>
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

    <br/>
    <br/>
    <br/>
    <button @click="goHome">Redirect to Home</button>

    <br/>
    <br/>
    <br/>
    <button @click="handleLogin">handleLogin</button>
    <button @click="handCreate">handleCreate</button>
</template>

<script>
import { onMounted, reactive } from "vue"; // import this for create values reactive
import { useRouter } from 'vue-router'

import useModal from '../hooks/useModal'

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
    const router = useRouter()
    const modal = useModal()
    // not reactive
    const state = reactive({
      currentTasks: undefined,
      showList: false,
      tasks: [{ name: "Study vue3", isDone: false }],
    })

    onMounted( () => {
      console.log('onMounted now with new syntaxs')
    })
    
    function handleLogin(){
      modal.open({
        component: 'ModalLogin'
      })
    }

    function handleCreate(){
      modal.open({
        component: 'ModalCreate'
      })
    }

    function goHome (){
      router.push({name: 'Home'})
    }

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
      goHome,
      handleLogin,
      handleCreate
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
