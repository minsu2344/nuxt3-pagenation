<template>
  <div id="app">
    <link
      href="https://cdn.jsdelivr.net/npm/remixicon@2.2.0/fonts/remixicon.css"
      rel="stylesheet"
    />
    <div class="save-msg" :class="{ popup: autoSave }">임시 저장</div>
    <div class="title-container">
      <input v-model="title" type="text" placeholder="제목을 입력하세요." />
    </div>
    <Tiptap :content="content" :max-limit="280" @update-content="handleUpdateContent" />
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

const title = ref<string>("");
const content = ref<string>("");
const file = ref<File[]>([]);
const autoSave = ref<boolean>(false);
let timeout: any

// 임시저장 알림
watch(
  () => content.value,
  () => {
    if(timeout) clearTimeout(timeout);
    autoSave.value = false;
    if (content.value !== "<p></p>") {
      timeout = setTimeout(() => {
        autoSave.value = true;
        if (autoSave) setTimeout(() => (autoSave.value = false), 3000);
      }, 5000);
    }
  }
);

// 작성 완료 버튼 클릭
async function onSubmit() {
  if(timeout) clearTimeout(timeout);
  try {
    const formData = new FormData();

    for (let i = 0; i < file.value.length; i++) {
      formData.append("file", file.value[i]);
    }
    formData.append("title", title.value);
    formData.append("content", content.value);
    await axios.post("", formData, {
      headers: {
        "Content-Type": "multipart/form-data",
      },
    });
    alert("글 작성이 완료되었습니다.");
    router.push({ path: "/" });
  } catch (e) {
    alert("오류가 발생했습니다.");
    console.log(e);
  }
}

// 취소 버튼 클릭
function handleCancelWrite() {
  if (confirm("작성 중인 글은 저장되지 않습니다. 취소하시겠습니까?"))
    router.push({ path: "/" });
}

// emit: file
function handleUpdateFile(value: File[]) {
  file.value = value;
}

// emit: content
function handleUpdateContent(value: string) {
  content.value = value;
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

.title-container {
  margin: 2rem 0;

  input {
    width: 100%;
    height: 2rem;
    border-radius: 0.5rem;
    border: solid 2px #333;
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

.save-msg {
  position: fixed;
  height: 1.5rem;
  padding: 1rem 0.75rem;
  border-radius: 0.5rem;
  bottom: -3.5rem;
  left: 50%;
  transform: translate(-50%, 0);
  background-color: rgb(108, 214, 149);
  color: white;
  transition: all 0.2s ease-in-out;
  &.popup {
    bottom: 1.5rem;
  }
}
</style>
