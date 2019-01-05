<template>
    <div align="left" class="card todolist__card-create">
        <h5 class="todolist__card-title">Todo List</h5>
        <div class="todolist__btn-add" :style="hideBtnAdd" @click="showForm">
            <i class="icon ion-md-add-circle-outline"></i>
            <label class="todolist__label-create">Add Todo List</label>
        </div>
        <div class="card-body" v-show="isAdded === true">
            <form>
                <div class="form-group">
                    <input type="text"
                        class="form-control"
                        placeholder="Apa yang kamu kerjakan hari ini..."
                        :value="title"
                        @input="addTitle($event.target.value)"
                    >
                    <input type="text"
                        class="form-control category"
                        placeholder="Tambah category"
                        :value="category"
                        @input="addCategory($event.target.value)"
                    >
                </div>
                <input type="submit" class="btn btn-primary" @click.prevent="onClick">
            </form>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        title: String,
        category: String
    },
    data() {
        return {
            isAdded: false,
            taskTitle: this.title
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
        inputTitle(taskTitle) {
            this.$emit('input', taskTitle)
        },
        onClick() {
            if(!this.taskTitle === '') {
                return false
            } else {
                this.$emit('onClick')
                this.isAdded = false
                this.taskTitle = ''   
            }
        },
        addTitle(title) {
            this.$emit('input', title);
        },
        addCategory(category) {
            this.$emit('inputCategory', category)
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
    padding-left: 20px;
    color: dodgerblue
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
    width: auto;

}
.ion-md-add-circle-outline {
    font-size: 20px;
}
.todolist__label-create {
    vertical-align: middle
}
.form-control .category {
    background-color: transparent
}
@media (max-width: 350px) {
  
    .todolist__card-create {
        width: auto
    }
   
}
@media (max-width: 800px) {
    .todolist__card-create {
        width: auto
    }
}

</style>
