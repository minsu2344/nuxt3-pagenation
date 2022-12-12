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
    <div>
      <select class="contents-num" v-model="limit" @change="() => curPage = 1">
        <option value="3">3개씩 보기</option>
        <option value="5">5개씩 보기</option>
        <option value="10">10개씩 보기</option>
        <option value="15">15개씩 보기</option>
        <option value="20">20개씩 보기</option>
      </select>
    </div>
    <div class="page-container">
      <div class="prev-btn pagenation" @click="handlePrevBtn" v-if="curShowPage !== 0">◀</div>
      <div class="page-num pagenation" :class="{active: number === curPage}" v-for="number in computedCurShowPage" :key="number" @click="() => curPage = number">
        {{number}}
      </div>
      <div class="pagenation" v-if="maxPage > pageCount*(curShowPage+1)" @click="curPage = maxPage">...{{maxPage}}</div>
      <div class="next-btn pagenation" @click="handleNextBtn" v-if="curShowPage !== Math.floor(maxPage/pageCount)">▶</div>
    </div>
  </div>
</template>

<script setup lang="ts">
import {contentsData} from '@/data/data'

const router = useRouter();
const route = useRoute();

const limit = ref<number>(parseInt(route.query.limit as string) || 3); // 보여질 컨텐츠 개수
const curPage = ref<number>(parseInt(route.query.page as string) || 1); //  현재 페이지
const curShowPage = ref<number>(0); // 현재 보여질 페이지 index
const maxPage = ref<number>(Math.ceil(contentsData.length / limit.value)); // 최대 페이지
const pageCount: number = 5; // 보여질 페이지 개수

// 현재 보여질 페이지 리스트
const computedCurShowPage = computed(() => {
  const pageArr = []
  for(let i = curShowPage.value * pageCount + 1; i < (curShowPage.value+1) * pageCount + 1; i++) {
    if(i > maxPage.value) break;
    pageArr.push(i);
  }
  return pageArr;
})

// 현재 보여질 데이터
const computedShowData = computed(() => contentsData.slice(limit.value*(curPage.value-1), limit.value*(curPage.value)));

// 현재 페이지 바뀔 시 curShowPage 업데이트
watch(() => curPage.value, () => {
  if(isNaN(curPage.value)) {
    curPage.value = 1;
    return;
  }
  curShowPage.value = curPage.value%pageCount === 0 ? curPage.value/pageCount - 1 : Math.floor(curPage.value / pageCount);
  router.push({path: '/', query: {page: curPage.value, limit: limit.value}});
})
// limit 바뀔 시 
watch(() => limit.value, () => {
  if(isNaN(limit.value)) {
    limit.value = 3
    return;
  }
  maxPage.value = Math.ceil(contentsData.length / limit.value);
  router.push({path: '/', query: {page: curPage.value, limit: limit.value}});
});

// 쿼리 바뀔 때마다 값 변경
watch(() => route.query, () => {
  curPage.value = parseInt(route.query.page as string);
  limit.value = parseInt(route.query.limit as string);
})

onMounted(() => {
  router.push({path: '/', query:{page: curPage.value, limit: limit.value}});
})

// prev 버튼 눌렀을 때
function handlePrevBtn() {
  curPage.value = pageCount * curShowPage.value;
}

// next 버튼 눌렀을 때
function handleNextBtn() {
  curPage.value = pageCount * (curShowPage.value+1) + 1;
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