<template>
  <div>
    <header>
      <h1 class="p-8 m-8 font-bold text-3xl">Blog System App</h1>
    </header>
    <div v-if="isLoading" class="flex justify-center items-center h-screen">
      <v-progress-circular color="primary" indeterminate></v-progress-circular>
    </div>
    <div class="flex flex-wrap justify-between">
      <Card v-for="(post, index) in posts" :key="index" :posts="post" class="w-1/2 mb-4" />
    </div>
  </div>
</template>

<script>
import Card from '../components/CardComponent.vue'
import axiosInstance from '@/utils/api'

export default {
  components: {
    Card
  },
  data() {
    return {
      posts: [],
      isLoading: false
    }
  },
  mounted() {
    this.fetchData()
  },
  methods: {
    async fetchData() {
      try {
        this.isLoading = true
        setTimeout(async () => {
          const response = await axiosInstance.get('kumparan/terbaru')
          this.posts = response.data.data.posts
          this.isLoading = false
        }, 2000)
      } catch (error) {
        console.error('Error fetching data:', error)
        this.isLoading = false
      }
    }
  }
}
</script>
