<template>
  <div>
    <div v-if="isLoading" class="flex justify-center items-center h-screen">
      <v-progress-circular color="primary" indeterminate></v-progress-circular>
    </div>
    <div v-else-if="!isLoading && !details">
      <div class="p-3">
        <router-link :to="`/`"><button>Back</button></router-link>
      </div>
      <NoData />
    </div>
    <div v-else>
      <DetailComponent :details="details" />
    </div>
  </div>
</template>

<script>
import DetailComponent from '@/components/DetailComponent.vue'
import NoData from '@/components/NoData.vue'
import { detailsData } from '../utils/details.js'

export default {
  components: {
    DetailComponent,
    NoData
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
        await new Promise((resolve) => setTimeout(resolve, 1000))
        const key = this.$route.params.key
        this.details = detailsData.find((detail) => detail.key === key) || null
        this.isLoading = false
      } catch (error) {
        console.error('Error fetching data:', error)
        this.isLoading = false
      }
    }
  }
}
</script>
