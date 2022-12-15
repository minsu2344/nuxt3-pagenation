<template>
  <div id="app">
    <link
      href="https://cdn.jsdelivr.net/npm/remixicon@2.2.0/fonts/remixicon.css"
      rel="stylesheet"
    />

    <Tiptap v-model="content" :max-limit="280" />
    <input type="file" multiple @change="handleFileChange" />
    <button class="submit-button" @click="onSubmit">Submit</button>
  </div>
</template>

<script setup lang="ts">
import axios from "axios";

const router = useRouter();

const content = ref<string>("");
const file = ref([]);

function handleFileChange(e: any): void {
  file.value = e.target.files;
}

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