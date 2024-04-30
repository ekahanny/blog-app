<template>
  <div>
    <div v-if="isLoading" class="flex justify-center items-center h-screen">
      <v-progress-circular color="primary" indeterminate></v-progress-circular>
    </div>
    <!-- <NoData v-else-if="!isLoading" /> -->
    <div v-else><DetailComponent :details="details" /></div>
  </div>
</template>

<script>
import DetailComponent from '@/components/DetailComponent.vue'
import axiosInstance from '@/utils/api'
// import NoData from '../components/NoData.vue'

export default {
  components: {
    DetailComponent
    // NoData
  },
  data() {
    return {
      details: {},
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
        const key = this.$route.params.key
        const response = await axiosInstance.get(`/api/detail/${key}`)
        this.details = response.data.results
        this.isLoading = false
      } catch (error) {
        console.error('Error fetching data:', error)
        this.isLoading = false
      }
    }
  }
}
</script>
