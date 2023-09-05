<template>
  <div class=""> 
    <input v-model="taskList.newTask" placeholder="Новая задача"/>
    <div @click="handleAddTask()">Добавить</div>
  </div>
  <ToDoListItem
    v-for="item in taskList.tasks"
    v-bind:task="item"
    :onCompletedChange="handleCompletedChange"
    :onEdit="handleEditClick"
    :onDelete="handleDeleteTask"
    :onChangeDeadline="handleChangeDeadline"
    :key="item.id"
  />
</template>

<script>
  import ToDoListItem from './ToDoListItem.vue'
  import { taskList } from '../store/store.js'
  import { getToDoListAPI } from '../api/getToDoListAPI'

  const getToDoList = async () => {
    const tasks = await getToDoListAPI()
    taskList.tasks = tasks.map(item => ({
      ...item,
      isEdit: false,
      deadline: '',
      isExpired: false
    }))
  }

  const handleAddTask = () => {
    taskList.tasks.push({
      id: Math.round(taskList.tasks.length * Math.random() * 1000),
      title: taskList.newTask,
      completed: false,
      isEdit: false,
      deadline: '',
      userId: Math.round(Math.random() * 1000),
      isExpired: false
    })
  }

  const handleEditClick = (task) => {
    task.isEdit = !task.isEdit
  } 

  const handleCompletedChange = (task) => {
    task.completed = !task.completed
  }

  const handleDeleteTask = (id) => {
    const newList = taskList.tasks.filter(item => item.id !== id)
    taskList.tasks = newList
  }

  const handleChangeDeadline = (task) => {
    task.isExpired = Math.floor(new Date(task.deadline).getTime() / 1000) < Math.floor(Date.now() / 1000)
  }

  export default {
    created() {
      getToDoList()
    },
    name: "ToDoList",
    components: {
      ToDoListItem
    },
    defineComponent: {
      ToDoListItem
    },
    data() {
      return { taskList };
    },
    methods: {
      handleCompletedChange,
      handleDeleteTask,
      handleEditClick,
      handleAddTask,
      handleChangeDeadline
    }
  };
</script>