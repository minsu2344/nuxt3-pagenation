<template>
  <div id="app">
    <link
      href="https://cdn.jsdelivr.net/npm/remixicon@2.2.0/fonts/remixicon.css"
      rel="stylesheet"
    />

    <Tiptap v-model="content" :max-limit="280" />
    
    <button class="submit-button" @click="onSubmit">Submit</button>
  </div>
</template>

<script setup lang="ts">

const content = ref<string>('');

function onSubmit() {
  alert(JSON.stringify(content.value))
}
</script>

<style lang="scss" scoped>
#app {
  font-family: Arial;
  font-size: 16px;
  margin: 0 2rem;

  .submit-button {
    border: 1px solid #333;
    border-radius: 2px;
    background: #fff;
    color: #333;
    font-size: 16px;
    padding: 6px 12px;
    margin-top: 8px;
    -webkit-appearance: none;
    cursor: pointer;

    &:hover {
      background: #333;
      color: #fff;
    }
  }
}
</style>