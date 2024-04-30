<template>
  <div class="container mx-auto p-3">
    <div>
      <router-link :to="`/`"
        ><button>
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="currentColor"
            class="w-6 h-6"
          >
            <path stroke-linecap="round" stroke-linejoin="round" d="M15.75 19.5 8.25 12l7.5-7.5" />
          </svg></button
      ></router-link>
    </div>
    <h1 class="text-xl text-center">{{ details.title }}</h1>
    <p class="text-gray-500 mb-2">{{ details.date }}</p>
    <div v-html="renderMarkdown(details.content)"></div>

    <div class="space-y-2">
      <div class="flex space-x-2 mt-5 items-center">
        <p>Kategori</p>
        <div v-for="(category, index) in details.categories" :key="index">
          <v-chip color="primary">{{ category }}</v-chip>
        </div>
      </div>
      <div>
        <p>Tim Penulis: {{ details.author }}</p>
      </div>
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
              renderedContent += `<img src="${item}" alt="Image" class="mx-auto my-4 w-1/3">`
            } else if (item.includes('youtube.com') || item.includes('youtu.be')) {
              const videoId = this.getYouTubeVideoId(item)
              if (videoId) {
                renderedContent += `<div class="flex items-center justify-center embed-responsive ">
                                      <iframe src="https://www.youtube.com/embed/${videoId}" frameborder="0" allowfullscreen class="embed-responsive-item w-1/3 aspect-video"></iframe>
                                    </div>`
              }
            }
          } else {
            renderedContent += `<div class="prose max-w-none">${md.render(item)}</div>`
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
