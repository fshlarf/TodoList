<template>
  <div class="container">
    <div class="card">
      <create-list 
        @onClick="addTodoList"
        v-model="inputTitle"
      ></create-list>
      <div :class="{'todolist__container-todolist': isListAvailabled === true}">
        <div align="center" v-for="list in lists" :key="list.key">
          <todolist-card 
            v-show="list !== ''"
            :todo-title="list"
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
      inputTitle: '',
      lists: [],
      isListAvailabled: false
    }
  },
  components: {
    todolistCard: CardList,
    createList: CreateTodoList
  },
  methods: {
    addTodoList(inputTitle) {
      const newListTitle = this.inputTitle
      this.lists.push(newListTitle)
      this.isListAvailabled = true
    }
  }
}
</script>

<style>
.todolist__container-todolist {
  padding-bottom: 1rem;
}
</style>
