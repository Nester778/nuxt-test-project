<template>
  <div class="mt-10">
    <div v-if="loading">
      <div class="grid gap-6 px-5" :style="gridStyle">
        <div v-for="n in 8" :key="n" class="news-item rounded-2xl bg-gray-200 dark:bg-gray-700 p-6 flex flex-col relative">
          <div class="flex justify-center">
            <USkeleton class="h-[150px] w-[150px] rounded-xl bg-gray-300 dark:bg-gray-700" />
          </div>
          <div class="p-4 flex-1 space-y-3">
            <USkeleton class="h-6 w-[80%] sm:w-[250px] bg-gray-300 dark:bg-gray-700" />
            <USkeleton class="h-4 w-[70%] sm:w-[200px] bg-gray-300 dark:bg-gray-700" />
            <USkeleton class="h-6 w-[80%] sm:w-[250px] bg-gray-300 dark:bg-gray-700" />
            <USkeleton class="h-4 w-[70%] sm:w-[200px] bg-gray-300 dark:bg-gray-700" />
            <USkeleton class="h-6 w-[80%] sm:w-[250px] bg-gray-300 dark:bg-gray-700" />
            <USkeleton class="h-4 w-[70%] sm:w-[200px] bg-gray-300 dark:bg-gray-700" />
          </div>
        </div>
      </div>
    </div>

    <div v-else>
      <div class="grid gap-6 px-5" :style="gridStyle">
        <div v-for="news in newsList" :key="news.title" class="news-item rounded-2xl bg-gray-50 dark:bg-gray-700 p-6 flex flex-col relative">
          <div class="flex justify-center">
            <img class="rounded-xl w-40 h-40 object-cover" :src="news.picture" alt="News Image" />
          </div>
          <div class="mt-4 flex-1 group">
            <h2 class="text-xl font-semibold text-gray-800 dark:text-gray-100 relative">
              {{ news.title }}
              <span class="absolute left-0 bottom-0 h-0.5 w-0 bg-custom-line transition-all group-hover:w-full"></span>
            </h2>
            <p class="mt-2 text-gray-600 dark:text-gray-100 line-clamp-4">{{ news.text }}</p>
          </div>
          <small class="absolute bottom-3 right-4 text-gray-400">Published: {{ formatDate(news.date) }}</small>
        </div>
      </div>

      <div class="mt-15 mb-10 flex justify-center">
        <UPagination v-model:page="page" :total="allNewsList.length" :itemsPerPage="itemsPerPage" @click="update"
        color="primary"
        active-color="primary"
        size="xl"
        class="custom-pagination border-solid border-1 border-gray-300 dark:border-gray-700 dark:bg-gray-700"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
const allNewsList = ref([])
const newsList = ref([])
const loading = ref(true)
const itemsPerPage = 8

const fetchNews = async () => {
  try {
    const response = await fetch('http://localhost:3000/api/news')
    allNewsList.value = await response.json()
    newsList.value = allNewsList.value.slice(0, itemsPerPage)
  } catch (error) {
    console.error('Error fetching news:', error)
  } finally {
    loading.value = false
  }
}

const page = ref(1)
const update = () => {
  newsList.value = allNewsList.value.slice((page.value - 1) * itemsPerPage, page.value * itemsPerPage)
}

const formatDate = (date) => {
  return new Date(date).toLocaleDateString()
}

const gridStyle = computed(() => {
  return {
    gridTemplateColumns: 'repeat(auto-fill, minmax(300px, 1fr))',
  }
})

fetchNews()
</script>

<style scoped>
.news-item {
  min-height: 400px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  transition: transform 0.3s, box-shadow 0.3s;
  position: relative;
  cursor: pointer;
}


.news-item:hover {
  transform: translateY(-5px);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
}

.news-item:hover .group .bg-custom-line {
  width: 100%;
}

.line-clamp-4 {
  display: -webkit-box;
  -webkit-line-clamp: 4;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
}

.bg-custom-line {
  width: 0;
  height: 2px;
  background-color: oklch(70.4% 0.14 182.503);
  transition: width 0.3s ease-in-out;
}

.custom-pagination {
  color: #374151;
  border-radius: 8px;
  padding: 8px 12px;
  transition: background-color 0.3s ease, color 0.3s ease;
}

@media (max-width: 768px) {
  .news-item .p-4 .h-6, .news-item .p-4 .h-4 {
    width: 90%;
  }
}
</style>
