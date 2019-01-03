<template>
  <div class="container">
    <div class="card todolist__container-card">
      <create-list 
        @onClick="addTodoList"
        v-model="newList"
      ></create-list>
      <div :class="{'todolist__container-todolist': isListAvailabled === true}">
        <div align="center" v-for="list in lists.slice().reverse()" :key="list.index">
          <todolist-card 
            v-show="list !== ''"
            :todo-title="list"
            @onClick="deleteList"
          ></todolist-card>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CardList from '~/components/CardList.vue'
import CreateTodoList from '~/components/CreateTodoList.vue'

export default {
  data() {
    return {
      // inputTitle: '',
      lists: [],
      newList: null,
      isListAvailabled: false
    }
  },
  components: {
    todolistCard: CardList,
    createList: CreateTodoList
  },
  mounted() {
    if (localStorage.getItem('lists')) {
      try {
        this.lists = JSON.parse(localStorage.getItem('lists'));
      } catch(e) {
        localStorage.removeItem('lists');
      }
    }
  },
  methods: {
    addTodoList() {
      if(!this.newList) {
        return
      }

      this.lists.push(this.newList)
      this.newList = ''
      this.saveLists()
      console.log(this.lists)
    },
    saveLists() {
      const parsed = JSON.stringify(this.lists)
      localStorage.setItem('lists', parsed)
    },
    deleteList (x) {
      this.lists.splice(x, 1)
      this.saveLists()
      console.log(this.lists)
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
