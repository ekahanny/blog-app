<template>
  <div>
    <header>
      <h1 class="p-5 font-bold text-3xl">Tech News</h1>
    </header>
    <div v-if="isLoading" class="flex justify-center items-center h-screen">
      <v-progress-circular color="primary" indeterminate></v-progress-circular>
    </div>
    <NoData v-else-if="!isLoading && posts.length === 0" />
    <div v-else>
      <div class="md:flex md:flex-wrap md:justify-between">
        <Card v-for="(post, index) in posts" :key="index" :posts="post" class="w-1/2 mb-4" />
      </div>
    </div>
  </div>
</template>

<script>
import Card from '../components/CardComponent.vue'
import NoData from '../components/NoData.vue'
import { postsData } from '@/utils/posts.js'

export default {
  components: {
    Card,
    NoData
  },
  data() {
    return {
      posts: [],
      isLoading: false
    }
  },
  async mounted() {
    await this.fetchData()
  },
  methods: {
    async fetchData() {
      try {
        this.isLoading = true
        await new Promise((resolve) => setTimeout(resolve, 1000))
        this.posts = await Promise.resolve(postsData)
        this.isLoading = false
      } catch (error) {
        console.error('Error fetching data:', error)
        this.isLoading = false
      }
    }
  }
}
</script>
