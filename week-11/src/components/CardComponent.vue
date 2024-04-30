<template>
  <v-card class="mx-auto" max-width="400" hover>
    <v-img class="align-end text-white" height="200" :src="posts.thumbnail" cover>
      <v-card-title>{{ posts.title }}</v-card-title>
    </v-img>

    <v-card-subtitle class="pt-4">{{ posts.pubDate }}</v-card-subtitle>

    <v-card-text>
      <div>{{ posts.description }}</div>
    </v-card-text>

    <v-card-actions>
      <v-btn color="orange" text="Show Details" :href="posts.link"></v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  props: {
    posts: {
      type: Object,
      required: true
    }
  }
}
</script>
