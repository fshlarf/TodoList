<template>
  <div class="container newclass">
    <div class="card todolist__container-card">
      <create-task 
        @onClick="addTodoList"
        :title="newTask"
        @input="newTask = arguments[0]"
        :category="newCategory"
        @inputCategory="newCategory = arguments[0]"
      ></create-task>
      <div :class="{'todolist__container-todolist': isListAvailabled === true}" >
        <drop-down
          @onClickAll="filterByStatus(tasks, 'all')"
          @onClickDone="filterByStatus(tasks, 'true')"
          @onClickNotdone="filterByStatus(tasks, 'false')"
        ></drop-down>
        <div
          class="todolist__task-container"
          align="center" 
          v-for="(task, i) in tasks" 
          :key="i"
          draggable="true" 
          @dragstart="dragStart(i, $event)" 
          @dragover.prevent
          @dragend="dragEnd" 
          @drop="dragFinish(i, $event)"
        >
          <card-task 
            v-show="task.statusTask "
            :task-title="task.title"
            :task-category="task.category"
            @onClickDelete="deleteTask(task,i)"
            @onClickSubmitEdit="submitEdit(task,i)"
            @onClickEdit="showFormEdit(task, i)"
            v-model="updateTask"
            @keyupEnter="submitEdit(task,i)"
            :place-holder="task.title"
            :show-form="task.editable === true"
            :show-btnsubmit="task.editable === true"
            :show-btnedit="task.editable === false"
            v-bind:status="task.statusTask"
            @addStatus="changeStatus(task, i)"
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
      this.tasks = this.tasksAll.slice().reverse()
      this.newTask = ''
      this.newCategory = ''
      this.saveTasks()
    },
    saveTasks () {
      const parsed = JSON.stringify(this.tasksAll)
      localStorage.setItem('tasks', parsed)
    },
    deleteTask (task, i) {
      this.tasks.splice(this.tasks.indexOf(task), 1)
      this.saveTasks()
    },
    submitEdit (task, i) {
      if (this.updateTask === '') {
        task.editable = false
      } else {
        task.title = this.updateTask
        this.updateTask = ''
        task.editable = false
        this.saveTasks()
      }
    },
    showFormEdit(task, i) {
      task.editable = true
    },
    changeStatus(task, i) {
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
    dragEnd(ev) {
      this.dragging = -1
    },
    dragFinish(to, ev) {
      this.moveItem(this.dragging, to);
      this.saveTasks()
    },
    moveItem(from, to) {
      if (to === -1) {
        this.removeItemAt(from);
      } else {
        this.tasks.splice(to, 0, this.tasks.splice(from, 1)[0]);
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
.todolist__task-container {
  cursor: grab
}
</style>
