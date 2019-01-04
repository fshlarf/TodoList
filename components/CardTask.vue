<template>
    <div align="left" class="card todolist__card-list">
        <div class="card-body todolist__card-list">
            <h5 class="card-title" >{{ todoTitle }}</h5>
            <input type="text" class="form-control" @input="handleInput" v-model="content" :placeholder="placeHolder" v-show="showForm">
            <input type="checkbox" :value="status" @input="addStatus($event.target.value)">
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
        status: Boolean
    },
    data() {
        return {
            value: this.status,
            content: this.editTitle,
            thePlaceHolder: this.placeHolder
        }
    },
    methods: {
        onClickDelete () {
            this.$emit('onClickDelete')
        },
        onClickSubmitEdit () {
            this.$emit('onClickSubmitEdit')
        },
        onClickEdit () {
            this.$emit('onClickEdit')
        },
        handleInput(e) {
            this.$emit('input', this.content)
        },
        addStatus(status) {
            this.$emit('input', status)
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


/* Base for label styling
[type="checkbox"]:not(:checked),
[type="checkbox"]:checked {
  position: absolute;
  left: -9999px;
}
[type="checkbox"]:not(:checked) + label,
[type="checkbox"]:checked + label {
  position: relative;
  padding-left: 1.95em;
  cursor: pointer;
}

[type="checkbox"]:not(:checked) + label:before,
[type="checkbox"]:checked + label:before {
  content: '';
  position: absolute;
  left: 0; top: 0;
  width: 1.25em; height: 1.25em;
  border: 2px solid #ccc;
  background: #fff;
  border-radius: 4px;
  box-shadow: inset 0 1px 3px rgba(0,0,0,.1);
}
[type="checkbox"]:not(:checked) + label:after,
[type="checkbox"]:checked + label:after {
  content: '\2713\0020';
  position: absolute;
  top: .15em; left: .22em;
  font-size: 1.3em;
  line-height: 0.8;
  color: #09ad7e;
  transition: all .2s;
  font-family: 'Lucida Sans Unicode', 'Arial Unicode MS', Arial;
}
[type="checkbox"]:not(:checked) + label:after {
  opacity: 0;
  transform: scale(0);
}
[type="checkbox"]:checked + label:after {
  opacity: 1;
  transform: scale(1);
} */


</style>
