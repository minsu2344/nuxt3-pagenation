<template>
  <div class="container">
    <div class="box red"></div>
    <div class="box orange"></div>
    <div class="box yellow"></div>
    <div class="box green"></div>
    <div class="box blue"></div>
    <div class="box navy"></div>
    <div class="box purple"></div>
    <div class="box black"></div>
    <div class="box red"></div>
    <div class="box orange"></div>
    <div class="box yellow"></div>
    <div class="box green"></div>
    <div class="box blue"></div>
    <div class="box navy"></div>
    <div class="box purple"></div>
    <div class="box black"></div>
    <div class="box red"></div>
    <div class="box orange"></div>
    <div class="box yellow"></div>
    <div class="box green"></div>
    <div class="box blue"></div>
    <div class="box navy"></div>
    <div class="box purple"></div>
    <div class="box black"></div>
  </div>
</template>

<script setup lang="ts">
const container: Element | null = document.querySelector('.container');
let lastBox: Element | null = document.querySelector('box:last-child');
let count: number | undefined = container?.children.length;

const io = new IntersectionObserver((entry, observer) => {
  const ioTarget = entry[0].target;

  if(entry[0].isIntersecting) {
    console.log('현재 보이는 타겟', ioTarget);
    io.unobserve(lastBox!);

    lastBox = container
  }
})
</script>

<style scoped lang="scss">
.container {
  display: flex;
  flex-direction: column;
}

.box {
  width: 100px;
  height: 100px;
  margin-bottom: 2rem;
}

.red {
  background-color: red;
}

.orange {
  background-color: orange;
}

.yellow {
  background-color: yellow;
}

.green {
  background-color: green;
}

.blue {
  background-color: blue;
}

.navy {
  background-color: navy;
}

.purple {
  background-color: purple;
}

.black {
  background-color: black;
}
</style>