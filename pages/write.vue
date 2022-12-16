<template>
  <div id="app">
    <link
      href="https://cdn.jsdelivr.net/npm/remixicon@2.2.0/fonts/remixicon.css"
      rel="stylesheet"
    />
    <Tiptap v-model="content" :max-limit="280" />
    <div class="buttons">
      <input type="file" multiple accept="image/*" @change="handleFileChange" />
      <button class="submit-button" @click.prevent="onSubmit">Submit</button>
    </div>
    <div v-if="prevImage.length" class="preview-imgs">
      <img
        v-for="(src, i) in prevImage"
        :key="i"
        :src="src"
        alt="prev img"
        class="prev-img"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import axios from "axios";
import { FileData } from "@/store/file";

const router = useRouter();

const content = ref<string>("");
const file = ref<File[]>([]);

function handleFileChange(e: any): void {
  file.value = e.target.files;
}

const prevImage = computed(() => {
  const imgArr: string[] = [];
  for (let i = 0; i < file.value.length; i++) {
    if (file.value[i].type.includes("image"))
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
</script>

<style lang="scss" scoped>
#app {
  font-family: Arial;
  font-size: 16px;
  margin: 0 1rem;

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
  input {
  }
  margin-top: 0.5rem;
}

.prev-img {
  width: 8rem;
  height: 8rem;
}
</style>
