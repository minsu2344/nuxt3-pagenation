<template>
  <div>
    <div class="add-image">
      <div>
        <label for="upload">이미지 추가</label>
        <input
          id="upload"
          value=""
          type="file"
          multiple
          accept="image/*"
          @change="handleFileChange"
        />
      </div>
    </div>
    <div v-if="prevImage.length" class="prev-imgs-container">
      <div v-for="(src, i) in prevImage" :key="i" class="prev-imgs">
        <img :src="src" alt="prev img" class="prev-img" />
        <p class="delete" @click="(e) => handleDeleteClick(e, i)">x</p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
const props = defineProps({
  file: {
    type: Array,
    default: [] as File[],
  },
});

const file = ref<File[]>(props.file as File[]);

const emit = defineEmits<{
  (e: "update-file", value: File[]): void;
}>();

watch(
  () => file.value,
  () => emit("update-file", file.value)
);

function handleFileChange(e: any): void {
  if (!e.target.files.length) return;
  for (let i = 0; i < e.target.files.length; i++) {
    if (!e.target.files[i].type.includes("image")) {
      alert("이미지 파일이 아닙니다.");
      return;
    }
  }
  file.value.push(...e.target.files);
  e.target.value = "";
}

const prevImage = computed(() => {
  const imgArr: string[] = [];
  for (let i = 0; i < file.value.length; i++) {
    imgArr.push(URL.createObjectURL(file.value[i]));
  }
  return imgArr;
});

function handleDeleteClick(e: any, i: number): void {
  file.value.splice(i, 1);
}
</script>

<style scoped lang="scss">
.add-image {
  display: flex;
  justify-content: space-between;
  align-items: center;
  label {
    cursor: pointer;
    border: solid #333 1px;
    border-radius: 0.5rem;
    padding: 0.5rem;
    transition: all linear 0.2s;
    &:hover {
      background-color: #333;
      color: white;
    }
  }
  input {
    display: none;
  }
  margin-top: 1rem;
}

.prev-img {
  width: 8rem;
  height: 8rem;
  padding: 0.25rem;
  border: solid 2px gray;
  border-radius: 0.5rem;
}

.prev-imgs-container {
  display: flex;
  flex-wrap: wrap;
  margin-top: 1rem;
}

.prev-imgs {
  position: relative;
  color: red;
  font-size: 1.25rem;
  & + & {
    margin-left: 0.5rem;
  }
}

.delete {
  position: absolute;
  right: 0.5rem;
  top: 0.25rem;
  margin: 0;
  cursor: pointer;
}
</style>
