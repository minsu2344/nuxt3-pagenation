<template>
  <div class="home">
    <div class="title">
      <h2>HOME</h2>
    </div>
    <div class="line" />
    <div class="contents">
      <div class="contents-title">
        <p>No</p>
        <p>제목</p>
        <p>글쓴이</p>
        <p>작성 시간</p>
        <p>좋아요</p>
      </div>
      <div class="contents-main" v-for="data in contentsData" :key="data.id">
        <p>{{data.id}}</p>
        <nuxt-link :to="`/contents/${data.id}`">{{data.title}}</nuxt-link>
        <p>{{data.author}}</p>
        <p>{{data.createdAt}}</p>
        <p>{{data.like}}</p>
      </div>
    </div>
    <div>
      <select class="contents-num" v-model="limit">
        <option value="5">5개씩 보기</option>
        <option value="10">10개씩 보기</option>
        <option value="15">15개씩 보기</option>
        <option value="20">20개씩 보기</option>
      </select>
    </div>
    <div class="page-container">
      <div class="prev-btn pagenation">◀</div>
      <div class="page-num pagenation" :class="{active: number === curPage}" v-for="number in computedCurShowPage" :key="number">
        {{number}}
      </div>
      <div class="next-btn pagenation">▶</div>
    </div>
  </div>
</template>

<script setup lang="ts">
import {contentsData} from '@/data/data'

const limit = ref<number>(5);
const maxPage = ref<number>(Math.ceil(contentsData.length / limit.value));
const curPage = ref<number>(1);
const curShowPage = ref<number>(1);

const computedCurShowPage = computed(() => curShowPage.value * limit.value);
const computedShowData = computed(() => contentsData.splice)
</script>

<style scoped lang="scss">
.home {
  margin: 0 2rem;
}

h2 {
  font-size: 3rem;
}

.title {
  margin-bottom: 1rem;
}

.line {
  border-bottom: 0.25rem solid black;
}

.contents {
  display: flex;
  flex-direction: column;
}

.contents > div {
  display: grid;
  grid-template-columns: 1fr 5fr 2fr 3fr 1fr;
  text-align: center;
  border-bottom: 0.125rem solid gray;
  padding: 0.5rem 0;
}

.contents-title {
  font-weight: bold;
}

.page-container {
  display: flex;
  justify-content: center;
  align-items: center;
}

.pagenation {
  & + & {
    margin-left: 0.5rem;
  }
  font-size: 1.25rem;
  cursor: pointer;
}

.page-num {
  &:hover {
    text-decoration: underline;
  }
}

.active {
  font-weight: bold;
  font-size: 1.5rem;
}
</style>