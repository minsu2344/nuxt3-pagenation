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
      <div class="contents-main" v-for="data in computedShowData" :key="data.id">
        <p>{{data.id}}</p>
        <nuxt-link :to="`/contents/${data.id}`">{{data.title}}</nuxt-link>
        <p>{{data.author}}</p>
        <p>{{data.createdAt}}</p>
        <p>{{data.like}}</p>
      </div>
    </div>
    <Pagenation
      @update-limit="handleUpdateLimit"
      @update-cur-page="handleUpdateCurPage"
    />
  </div>
</template>

<script setup lang="ts">
import {contentsData} from '@/data/data'

const router = useRouter();
const route = useRoute();

const limit = ref<number>(3);
const curPage = ref<number>(1);

// 현재 보여질 데이터
const computedShowData = computed(() => contentsData.slice(limit.value*(curPage.value-1), limit.value*(curPage.value)));

// limit emit 함수
function handleUpdateLimit(value: number): void {
  limit.value = value;
}
//  curPage emit 함수
function handleUpdateCurPage(page: number): void {
  curPage.value = page;
}
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
</style>