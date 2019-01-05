<template>
    <div class="">
      <div class="dropdown">
        <button class="dropbtn">Filter</button>
            <div class="dropdown-content">
                <a @click="onClickAll">All</a>
                <a @click="onClickDone">Done</a>
                <a @click="onClickNotdone">Not Done</a>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    props: {

    },
    methods: {
        onClickAll() {
            this.$emit('onClickAll')
        },
        onClickDone() {
            this.$emit('onClickDone')
        },
        onClickNotdone() {
            this.$emit('onClickNotdone')
        },
    }
}
</script>

<style >
.dropbtn {
    background-color: #dfe3e6;
    color: black;
    padding: 12px;
    font-size: 16px;
    border: 0.3px solid rgb(174, 174, 174);
    cursor: pointer;
    width: 23.9rem;
}

.dropdown {
    position: relative;
    display: inline-block;
    margin-bottom: 1rem;
}

.dropdown-content {
    display: none;
    position: absolute;
    background-color: #f9f9f9;
    min-width: 160px;
    box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
    z-index: 1;
    width: 23.9rem;

}

.dropdown-content a {
    color: black;
    padding: 12px 16px;
    text-decoration: none;
    display: block;
}

.dropdown-content a:hover {background-color: #f1f1f1}

.dropdown:hover .dropdown-content {
    display: block;
}

.dropdown:hover .dropbtn {
    background-color: #9facb3a5;
}
</style>

