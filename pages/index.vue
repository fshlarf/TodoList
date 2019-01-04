<template>
  <div class="container">
    <div class="card todolist__container-card">
      <create-task 
        @onClick="addTodoList"
        v-bind:title="newTask"
        v-on:input="newTask = arguments[0]"
      ></create-task>
      <div :class="{'todolist__container-todolist': isListAvailabled === true}">
        <div align="center" v-for="(task, index) in tasks.slice().reverse()" :key="index">
          <card-task 
            v-show="task !== ''"
            :todo-title="task.title"
            @onClickDelete="deleteList(task,index)"
            @onClickSubmitEdit="submitEdit(task,index)"
            @onClickEdit="showFormEdit(task, index)"
            v-model="updateTask"
            :place-holder="task.title"
            :show-form="task.editable === true"
            :show-btnsubmit="task.editable === true"
            :show-btnedit="task.editable === false"
          ></card-task>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CardTask from '~/components/CardTask.vue'
import CreateTask from '~/components/CreateTask.vue'

export default {
  data() {
    return {
      tasks: [],
      newTask: null,
      categoryTask: null,
      isListAvailabled: false,
      updateTask: ''
    }
  },
  components: {
    cardTask: CardTask,
    createTask: CreateTask
  },
  mounted() {
    if (localStorage.getItem('tasks')) {
      try {
        this.tasks = JSON.parse(localStorage.getItem('tasks'));
      } catch(e) {
        localStorage.removeItem('tasks');
      }
    }
  },
  methods: {
    addTodoList () {
      if(!this.newTask) {
        return
      }
      this.tasks.push({
        title: this.newTask, 
        statusList: true,
        id: this.tasks.length,
        editable: false,
      })
      this.newTask = null
      this.saveTasks()
      console.log(this.tasks)
    },
    saveTasks () {
      const parsed = JSON.stringify(this.tasks)
      localStorage.setItem('tasks', parsed)
    },
    deleteList (task, index) {
      console.log(this.tasks.indexOf(task))
      this.tasks.splice(this.tasks.indexOf(task), 1)
      this.saveTasks()
    },
    submitEdit (task, index) {
      if (this.updateTask === '') {
        return false
      } else {
        task.title = this.updateTask
        this.updateTask = ''
        console.log(this.tasks)
        this.saveTasks()
        task.editable = false
      }
    },
    showFormEdit(task, index) {
      // this.updateTask = task.title
      task.editable = true
      console.log(this.updateTask)
    }
  }
}
</script>

<style>
.todolist__container-todolist {
  padding-bottom: 1px;
}
.todolist__container-card {
  background-color: #dfe3e6;
}
</style>
