<template>
  <div>
    <ArticleView :article="article" :author="author" />
  </div>
</template>

<script>
import global from '@/utils/global'
import getSiteMeta from '@/utils/getSiteMeta'
import ArticleView from '@/components/articles/ArticleView'

export default {
  components: {
    ArticleView,
  },
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
      title: this.article.title + ' | ' + global.siteName,
      meta: [
        ...this.meta,
        {
          property: 'article:published_time',
          content: this.article.date,
        },
        {
          property: 'article:modified_time',
          content: this.article.updatedAt,
        },
        {
          property: 'article:category',
          content: this.article.category
            ? this.article.category.toString()
            : '',
        },
        { name: 'twitter:label1', content: 'Written by' },
        { name: 'twitter:data1', content: global.author || '' },
        { name: 'twitter:label2', content: 'Filed under' },
        {
          name: 'twitter:data2',
          content: this.article.category
            ? this.article.category.toString()
            : '',
        },
      ],
      link: [
        {
          hid: 'canonical',
          rel: 'canonical',
          href: `${this.$config.baseUrl}/articles/${this.$route.params.slug}`,
        },
      ],
    }
  },
  computed: {
    meta() {
      const metaData = {
        type: 'article',
        title: this.article.title,
        description: this.article.description,
        url: `${this.$config.baseUrl}/articles/${this.$route.params.slug}`,
        mainImage: this.article.featureImage,
      }
      return getSiteMeta(metaData)
    },
  },
  methods: {
    formatDate(dateString) {
      const date = new Date(dateString)
      return date.toLocaleDateString() || ''
    },
  },
}
</script>
