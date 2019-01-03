<template>
    <div align="left" class="card todolist__card-create" style="width: 25rem;">
        <h5 class="todolist__card-title">Todo List</h5>
        <div class="todolist__btn-add" :style="hideBtnAdd" @click="showForm"> + Add Todo List</div>
        <div class="card-body" v-show="isAdded === true">
            <form>
                <div class="form-group">
                    <input class="form-control" placeholder="Apa yang kamu kerjakan hari ini..." v-model="content" @input="handleInput">
                </div>
                <input type="submit" class="btn btn-primary" @click.prevent="onClick">
            </form>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        listTitle: String,
    },
    data() {
        return {
            isAdded: false,
            content: this.listTitle
        }
    },
    computed: {
        hideBtnAdd() {
            return this.isAdded ? {'display' : 'none'} : ''
        }
    },
    methods: {
        showForm() {
            this.isAdded = true
            this.btnAdd = false
        } ,
        handleInput(e) {
            this.$emit('input', this.content)
        },
        onClick() {
            if(this.content === '') {
                return false
            } else {
                this.$emit('onClick')
                this.isAdded = false
                this.content = ''   
            }
        }
    }
}
</script>

<style>
.todolist__card-title {
    padding: 15px;
    padding-left: 18px;
    padding-bottom: 0px;
}
.todolist__btn-add {
    cursor: pointer;
    padding: 15px;
    padding-left: 18px;
}
.todolist__btn-add:hover {
    background-color: #9facb3a5;
}
.todolist__btn-add:active {
    background-color: #9facb3a5;
}
.card-body {
    padding-top: 10px;
}
.todolist__card-create {
    border-color: transparent;
  background-color: #dfe3e6;

}
</style>
