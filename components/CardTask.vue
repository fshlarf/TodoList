<template>
    <div align="left" class="card todolist__card-list">
        <div class="card-body todolist__card-list">
            <h5 class="card-title" >{{ todoTitle }}</h5>
            <input type="text" class="form-control" @input="handleInput" v-model="content" :placeholder="placeHolder" v-show="showForm">
            <div>
                <!-- <img src="../assets/icon/done-all/1x/done-all.png" alt="icon not found" :value="status" @click="addStatus"> -->
                <i class="icon ion-md-checkmark-circle-outline" v-bind:class="status === 'true' ? 'status-done' : ''" :value="status" @click="addStatus"></i>
                <label class="todolist__label" v-show="status === 'true'"><b><i>Done</i></b></label>
            </div>
            <div class="todolist__btn-delete" @click="onClickDelete">delete</div>
            <div class="todolist__btn-edit" @click="onClickSubmitEdit" v-show="showBtnsubmit">submit</div>
            <div class="todolist__btn-edit" @click="onClickEdit" v-show="showBtnedit">edit</div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        todoTitle: String,
        checked: Boolean,
        editTitle: String,
        placeHolder: String,
        showForm: Boolean,
        showBtnsubmit: Boolean,
        showBtnedit: Boolean,
        status: String
    },
    data() {
        return {
            value: this.status,
            content: this.editTitle,
            thePlaceHolder: this.placeHolder
        }
    },
    methods: {
        onClickDelete() {
            this.$emit('onClickDelete')
        },
        onClickSubmitEdit() {
            this.$emit('onClickSubmitEdit')
        },
        onClickEdit () {
            this.$emit('onClickEdit')
        },
        handleInput(e) {
            this.$emit('input', this.content)
        },
        addStatus() {
            this.$emit('addStatus')
        }
    }
}
</script>

<style>
.todolist__card-list {
    padding-bottom: 0;
    margin-bottom: 0.5rem;
    width: 24rem;
}
.todolist__btn-delete {
    cursor: pointer;
    float: right;
}
.todolist__btn-delete:hover {
    color: dodgerblue;
}
.todolist__btn-edit {
    cursor: pointer;
    float: right;
    margin-right: 10px;
}
.todolist__btn-edit:hover {
    color: dodgerblue;
}
.todolist__label {
    font-size: 14px;
    vertical-align: middle;
    color:green;
}
[type="checkbox"]:not(:checked),
[type="checkbox"]:checked {
    left: -9999px;
    cursor: pointer;
    left: 0; top: 0;
    width: 1.25em; height: 1.25em;
    border: 2px solid #ccc;
    background:white;
    border-radius: 4px;
    box-shadow: inset 0 1px 3px rgba(0,0,0,.1);
}
.ion-md-checkmark-circle-outline {
    color: #ccc;
    font-size: 24px;
    cursor: pointer;
}
.ion-md-checkmark-circle-outline:hover {
    color: #bbb;
    font-size: 26px;
    cursor: pointer;
}
.status-done {
    color: green;
    font-size: 26px;
    cursor: pointer;
}
.status-done:hover {
    color: green;
    font-size: 26px;
    cursor: pointer;
}

</style>
