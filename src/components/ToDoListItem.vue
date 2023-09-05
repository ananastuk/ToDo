<template>
  <div class="todo-list-item__wrapper">
    <input class="todo-list-item__complete" @change="onCompletedChange(task)" type="checkbox" :checked="task.completed" name="done"/>
    <del v-if="task.completed === true">{{task.title}}</del>
    <input v-if="task.isEdit === true" type="text" v-model="task.title"/>
    <div v-else>{{task.title}}</div>
    <button @click="onDelete(task.id)">Удалить</button>
    <button v-if="task.isEdit" @click="onEdit(task)">Сохранить</button>
    <button v-else @click="onEdit(task)">Редактировать</button>
  </div>
  <!-- <div>
    <input type="date" v-model="task.deadline" >
  </div> -->
</template>
  
<script>
  export default {
    name: "ToDoListItem",
    props: {
      task: Object,
      onCompletedChange: Function,
      onEdit: Function,
      onDelete: Function,
      onChangeDeadline: Function
    },
  };
  
</script>

<style>
  .todo-list-item__wrapper {
    display: flex;
  }
</style>