<template>
  <div id="app">
    <link
      href="https://cdn.jsdelivr.net/npm/remixicon@2.2.0/fonts/remixicon.css"
      rel="stylesheet"
    />
    <Tiptap v-model="content" :max-limit="280" />
    <div class="buttons">
      <div>
        <label for="upload">사진 선택</label>
        <input
          id="upload"
          value=""
          type="file"
          multiple
          accept="image/*"
          @change="handleFileChange"
        />
      </div>
      <button class="submit-button" @click.prevent="onSubmit">Submit</button>
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
import axios from "axios";

const router = useRouter();

const content = ref<string>("");
const file = ref<File[]>([]);

function handleFileChange(e: any): void {
  if (!e.target.files.length) return;
  for (let i = 0; i < e.target.files.length; i++) {
    if (!e.target.files[i].type.includes("image")) {
      alert("이미지 파일이 아닙니다. ★토스트★");
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

async function onSubmit() {
  const formData = new FormData();

  for (let i = 0; i < file.value.length; i++) {
    formData.append("file", file.value[i]);
  }
  formData.append("content", content.value);
  await axios.post("", formData, {
    headers: {
      "Content-Type": "multipart/form-data",
    },
  });
  alert("글 작성이 완료되었습니다.");
  router.push({ path: "/" });
}

function handleDeleteClick(e: any, i: number): void {
  file.value.splice(i, 1);
}
</script>

<style lang="scss" scoped>
#app {
  font-family: Arial;
  font-size: 16px;

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

.buttons {
  display: flex;
  justify-content: space-between;
  align-items: center;
  label {
    cursor: pointer;
  }
  input {
    display: none;
  }
  margin-top: 0.5rem;
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
