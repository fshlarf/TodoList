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
        <div 
          align="center" 
          v-for="(task, index) in tasks.slice().reverse()" 
          :key="index"
          draggable="true" 
          @dragstart="dragStart(i, $event)" 
          @dragover.prevent 
          @dragenter="dragEnter" 
          @dragleave="dragLeave" 
          @dragend="dragEnd" 
          @drop="dragFinish(i, $event)"
        >
          <card-task 
            v-show="task.statusTask "
            :task-title="task.title"
            :task-category="task.category"
            @onClickDelete="deleteTask(task,index)"
            @onClickSubmitEdit="submitEdit(task,index)"
            @onClickEdit="showFormEdit(task, index)"
            v-model="updateTask"
            @keyupEnter="submitEdit(task,index)"
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
  loading: false,
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
      tasksAll: [],
      dragging: -1
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
  computed: {
    isDragging() {
      return this.dragging > -1
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
      this.$nuxt.$loading.start()
        setTimeout(() => {
          this.tasks = status == 'all' ? this.tasksAll : this.tasksAll.filter(e => e.statusTask == status);
          this.$nuxt.$loading.finish()
        }, 1000)
    },
    dragStart(which, ev) {
      ev.dataTransfer.setData('Text', this.id);
      ev.dataTransfer.dropEffect = 'move'
      this.dragging = which;
    },
    dragEnter(ev) {
      /* 
      if (ev.clientY > ev.target.height / 2) {
        ev.target.style.marginBottom = '10px'
      } else {
        ev.target.style.marginTop = '10px'
      }
      */
    },
    dragLeave(ev) {
      /* 
      ev.target.style.marginTop = '2px'
      ev.target.style.marginBottom = '2px'
      */
    },
    dragEnd(ev) {
      this.dragging = -1
    },
    dragFinish(to, ev) {
      this.moveItem(this.dragging, to);
      ev.target.style.marginTop = '2px'
      ev.target.style.marginBottom = '2px'
    },
    moveItem(from, to) {
      if (to === -1) {
        this.removeItemAt(from);
      } else {
        this.todos.splice(to, 0, this.todos.splice(from, 1)[0]);
      }
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
