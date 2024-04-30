<template>
  <v-card class="mx-auto" max-width="400" hover>
    <v-img class="align-end text-white" height="200" :src="posts.thumb" cover>
      <v-card-title>{{ posts.title }}</v-card-title>
    </v-img>
    <v-card-subtitle class="pt-4">{{ posts.time }}</v-card-subtitle>
    <v-card-text class="flex custom-card-text">
      <p>{{ posts.author }}</p>
      <span class="mx-2">|</span>
      <p>{{ posts.tag }}</p>
    </v-card-text>
    <v-card-text>
      <div>{{ truncateDescription(posts.desc) }}</div>
    </v-card-text>
    <router-link :to="`/detail/${posts.key}`">
      <v-card-actions>
        <v-btn color="orange" text="Show Details" :href="posts.link"></v-btn>
      </v-card-actions>
    </router-link>
  </v-card>
</template>

<script>
export default {
  props: {
    posts: {
      type: Object,
      required: true
    }
  },
  methods: {
    truncateDescription(description) {
      if (description.length > 50) {
        return description.slice(0, 80) + '...'
      }
      return description
    }
  }
}
</script>

<style scoped>
.custom-card-text {
  padding-top: 2px;
  padding-left: 16px;
  padding-bottom: 0px;
}
</style>
