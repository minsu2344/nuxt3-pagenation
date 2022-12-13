<template>
  <div>
    <div class="top">
      <div class="title">
        <h1>Board</h1>
      </div>
      <div class="navigation">
        <nuxt-link to="/?page=1&limit=3">HOME</nuxt-link>
        <nuxt-link to="/notion">NOTION</nuxt-link>
      </div>
    </div>
      <slot />
  </div>
</template>

<script setup lang="ts">

</script>

<style scoped lang="scss">
.top {
  background-color: black;
  text-align: center;
  margin-bottom: 2rem;
}

.title {
  color: white;
  padding-top: 0.75rem;
  padding-bottom: 0.75rem;
}

.navigation {
  display: flex;
  justify-content: center;
  padding-bottom: 0.5rem;
}

a {
  color: white;
  &+& {
    margin-left: 3rem
  }
}
</style>