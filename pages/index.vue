<template>
  <div class="container">
    <div class="card todolist__container-card">
      <create-task 
        @onClick="addTodoList"
        :title="newTask"
        @input="newTask = arguments[0]"
        :category="newCategory"
        @inputCategory="newCategory = arguments[0]"

      ></create-task>
      <div :class="{'todolist__container-todolist': isListAvailabled === true}">
        <drop-down
          @onClickAll="filterByStatus(tasks, 'all')"
          @onClickDone="filterByStatus(tasks, 'true')"
          @onClickNotdone="filterByStatus(tasks, 'false')"
        ></drop-down>
        <div align="center" v-for="(task, index) in tasks.slice().reverse()" :key="index">
          <card-task 
            v-show="task.statusTask "
            :task-title="task.title"
            :task-category="task.category"
            @onClickDelete="deleteTask(task,index)"
            @onClickSubmitEdit="submitEdit(task,index)"
            @onClickEdit="showFormEdit(task, index)"
            v-model="updateTask"
            :place-holder="task.title"
            :show-form="task.editable === true"
            :show-btnsubmit="task.editable === true"
            :show-btnedit="task.editable === false"
            v-bind:status="task.statusTask"
            @addStatus="changeStatus(task, index)"
          ></card-task>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CardTask from '~/components/CardTask.vue'
import CreateTask from '~/components/CreateTask.vue'
import DropDown from '~/components/DropDown.vue'

export default {
  asyncData() {
    return new Promise((resolve) => {
      setTimeout(function () {
        resolve({})
      }, 1000)
    })
  },
  data() {
    return {
      tasks: [],
      newTask: null,
      categoryTask: null,
      isListAvailabled: false,
      updateTask: '',
      newCategory: '',
      tasksAll: []
    }
  },
  components: {
    cardTask: CardTask,
    createTask: CreateTask,
    dropDown: DropDown
  },
  mounted() {
    if (localStorage.getItem('tasks')) {
      try {
        this.tasksAll = JSON.parse(localStorage.getItem('tasks'));
        this.tasks = this.tasksAll   
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
      this.tasksAll.push({
        title: this.newTask,
        category: this.newCategory, 
        statusTask: 'false',
        id: this.tasks.length,
        editable: false,
      })
      this.tasks = this.tasksAll
      this.newTask = ''
      this.newCategory = ''
      this.saveTasks()
    },
    saveTasks () {
      const parsed = JSON.stringify(this.tasksAll)
      localStorage.setItem('tasks', parsed)
    },
    deleteTask (task, index) {
      this.tasks.splice(this.tasks.indexOf(task), 1)
      this.saveTasks()
    },
    submitEdit (task, index) {
      if (this.updateTask === '') {
        task.editable = false
      } else {
        task.title = this.updateTask
        this.updateTask = ''
        this.saveTasks()
        task.editable = false
      }
    },
    showFormEdit(task, index) {
      task.editable = true
    },
    changeStatus(task, index) {
      if (task.statusTask == 'true') {
        task.statusTask = 'false'
      } else if (task.statusTask == 'false') {
        task.statusTask = 'true'
      }
      this.saveTasks()
    },
    filterByStatus(tasks, status) {
      this.tasks = status == 'all' ? this.tasksAll : this.tasksAll.filter(e => e.statusTask == status);
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
  padding-left: 5px;
  padding-right: 5px; 
}

</style>
