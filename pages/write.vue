<template>
  <div id="app">
    <link
      href="https://cdn.jsdelivr.net/npm/remixicon@2.2.0/fonts/remixicon.css"
      rel="stylesheet"
    />
    <Tiptap v-model="content" :max-limit="280" />
    <ImgUpload :file="file" @update-file="handleUpdateFile" />
    <div class="navigation-buttons">
      <button class="cancel-button" @click="handleCancelWrite">취소</button>
      <button class="submit-button" @click.prevent="onSubmit">작성 완료</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import axios from "axios";

const router = useRouter();

const content = ref<string>("");
const file = ref<File[]>([]);

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

function handleCancelWrite() {
  if (confirm("작성 중인 글은 저장되지 않습니다. 취소하시겠습니까?"))
    router.push({ path: "/" });
}

function handleUpdateFile(value: File[]) {
  file.value = value;
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
    transition: all 0.2s linear;

    &:hover {
      background: #333;
      color: #fff;
    }
  }
}

.cancel-button {
  border: 1px solid #333;
  border-radius: 2px;
  background: #fff;
  color: #333;
  font-size: 16px;
  padding: 6px 12px;
  margin-top: 8px;
  -webkit-appearance: none;
  cursor: pointer;
  margin-right: 1rem;
  transition: all 0.2s linear;

  &:hover {
    background: #333;
    color: #fff;
  }
}

.navigation-buttons {
  display: flex;
  justify-content: center;
  margin-top: 1rem;
}
</style>
