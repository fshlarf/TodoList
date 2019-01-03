<template>
    <div align="left" class="card todolist__card-list">
        <div class="card-body todolist__card-list">
            <h5 class="card-title">{{ todoTitle }}</h5>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        todoTitle: String
    }
}
</script>

<style>
.todolist__card-list {
    padding-bottom: 0;
    margin-bottom: 0.5rem;
    width: 24rem;
}
</style>
