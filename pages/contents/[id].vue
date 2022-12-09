<template>
  <div>
    {{description}}
  </div>
</template>

<script setup lang="ts">
import { contentsData } from '~~/data/data';

const route = useRoute();
const router = useRouter();
const description: string | undefined = contentsData.find(data => data.id === parseInt(route.params.id as string))?.description

onMounted(() => description ?? router.push('/404'));
</script>

<style scoped>

</style>