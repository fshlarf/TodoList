<template>
  <div class="container">
    <p>About Page</p>
    <NuxtLink to="/home">
      Go to /
    </NuxtLink>
  </div>
</template>

<script>
export default {
  asyncData() {
    return new Promise((resolve) => {
      setTimeout(function () {
        resolve({})
      }, 1000)
    })
  }
}
</script>

<style scoped>

</style>