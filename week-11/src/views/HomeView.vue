<template>
  <div>
    <header>
      <h1 class="p-5 font-bold text-3xl">Tech News</h1>
    </header>
    <div v-if="isLoading" class="flex justify-center items-center h-screen">
      <v-progress-circular color="primary" indeterminate></v-progress-circular>
    </div>
    <NoData v-else-if="!isLoading && posts.length === undefined" />
    <div v-else>
      <div class="md:flex md:flex-wrap md:justify-between">
        <Card v-for="(post, index) in posts" :key="index" :posts="post" class="w-1/2 mb-4" />
      </div>
    </div>
    <div class="flex justify-center my-5">
      <v-pagination :length="totalPages" v-model="currentPage" @input="updateUrl"></v-pagination>
    </div>
  </div>
</template>

<script>
import Card from '../components/CardComponent.vue'
import NoData from '../components/NoData.vue'
import axiosInstance from '@/utils/api'

export default {
  components: {
    Card,
    NoData
  },
  data() {
    return {
      posts: [],
      isLoading: false,
      currentPage: 1,
      totalPages: 34
    }
  },
  async mounted() {
    await this.fetchDataFromUrl()
  },
  watch: {
    $route: 'fetchDataFromUrl',
    currentPage: {
      handler: 'fetchData',
      immediate: false
    }
  },
  methods: {
    async fetchData() {
      try {
        this.isLoading = true
        const response = await axiosInstance.get(`api/tech/news?page=${this.currentPage}`)
        this.posts = response.data
        this.isLoading = false
        this.updateUrl()
      } catch (error) {
        console.error('Error fetching data:', error)
        this.isLoading = false
      }
    },
    async fetchDataFromUrl() {
      const page = parseInt(this.$route.query.page) || 1
      this.currentPage = page
      await this.fetchData()
    },
    updateUrl() {
      const url = new URL(window.location.href)
      url.searchParams.set('page', this.currentPage)
      window.history.pushState({ path: url.href }, '', url.href)
    }
  }
}
</script>
