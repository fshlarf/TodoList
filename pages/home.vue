<template>
  <div class="container">
    <!-- <p class="p-home">Hello {{ name }}!</p> -->
    <NuxtLink class="nuxt-home" to="/">
      Go to todo list app
    </NuxtLink>
  </div>
</template>

<script>
export default {
  asyncData() {
    return new Promise((resolve) => {
      setTimeout(function () {
        resolve({ name: 'world' })
      }, 3000)
    })
  }
}
</script>

<style scoped>
.p-home {
    margin-bottom: 20px;
    position: fixed;
}
.nuxt-home {
    margin-top: 60px;
}
</style>
