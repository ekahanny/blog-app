<template>
  <div class="p-3">
    <div>
      <router-link :to="`/`"><button>Back</button></router-link>
    </div>
    <h1 class="text-xl text-center">{{ details.title }}</h1>
    <p>{{ details.date }}</p>

    <!-- Render content using markdown-it -->
    <div v-html="renderMarkdown(details.content)"></div>

    <div class="flex space-x-2">
      <div v-for="(category, index) in details.categories" :key="index">
        <v-chip color="primary">{{ category }}</v-chip>
      </div>
    </div>
    <div>
      <p>Tim Penulis: {{ details.author }}</p>
    </div>
  </div>
</template>

<script>
import MarkdownIt from 'markdown-it'

export default {
  props: {
    details: {
      type: Object,
      required: true
    }
  },
  methods: {
    renderMarkdown(content) {
      const md = new MarkdownIt()
      let renderedContent = ''
      if (!Array.isArray(content)) {
        content = [content]
      }

      for (const item of content) {
        if (typeof item === 'string') {
          if (item.startsWith('http')) {
            if (item.endsWith('.png') || item.endsWith('.jpg') || item.endsWith('.jpeg')) {
              renderedContent += `<img src="${item}" alt="Image">`
            } else if (item.includes('youtube.com') || item.includes('youtu.be')) {
              const videoId = this.getYouTubeVideoId(item)
              if (videoId) {
                renderedContent += `<iframe width="560" height="315" src="https://www.youtube.com/embed/${videoId}" frameborder="0" allowfullscreen></iframe>`
              }
            }
          } else {
            renderedContent += md.render(item)
          }
        }
      }
      return renderedContent
    },
    getYouTubeVideoId(url) {
      const match = url.match(
        /(?:youtu\.be\/|youtube\.com\/(?:embed\/|v\/|watch\?v=|watch\?.+&v=))([\w\\-]{11})/
      )
      return match ? match[1] : null
    }
  }
}
</script>
