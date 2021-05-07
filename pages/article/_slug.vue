<template>
  <div>
    <h2>{{ article.title }}</h2>
    <h6>{{ formatDate(article.date) }}</h6>
    <h6>{{ article.description }}</h6>
    <img :src="article.featureImage" alt="" />
    <h6>{{ author.name }}</h6>
    <nuxt-content :document="article" />
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    const article = await $content('articles', params.slug).fetch()
    const authorname = article.author[0].toLowerCase().split(' ').join('-')
    const author = await $content('authors', authorname).fetch()
    return {
      article,
      author,
    }
  },
  head() {
    return {
      title: this.article.title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.article.description,
        },
        // Open Graph
        { hid: 'og:title', property: 'og:title', content: this.article.title },
        {
          hid: 'og:description',
          property: 'og:description',
          content: this.article.description,
        },
        // Twitter Card
        {
          hid: 'twitter:title',
          name: 'twitter:title',
          content: this.article.title,
        },
        {
          hid: 'twitter:description',
          name: 'twitter:description',
          content: this.article.description,
        },
      ],
    }
  },
  methods: {
    formatDate(dateString) {
      const date = new Date(dateString)
      return date.toLocaleDateString() || ''
    },
  },
}
</script>
