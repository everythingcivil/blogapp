<template>
  <div>
    <li v-for="item of articles" :key="item.slug">
      <NuxtLink :to="/categories/ + item.slug">{{ item.title }}</NuxtLink>
    </li>
    <section id="prev-next">
      <nuxt-link :to="prevLink">Prev page</nuxt-link>
      <nuxt-link v-if="nextPage" :to="`/article/page/${pageNo + 1}`"
        >Next page</nuxt-link
      >
    </section>
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params, error }) {
    const numberOfArticles = 5
    const pageNo = parseInt(params.number)
    const articleList = await $content('articles')
      .only(['title', 'date', 'description', 'featureImage', 'slug'])
      .sortBy('date', 'asc')
      .limit(numberOfArticles)
      .skip((numberOfArticles - 1) * (pageNo - 1))
      .fetch()

    if (!articleList.length) {
      return error({ statusCode: 404, message: 'No articles found!' })
    }

    const nextPage = articleList.length === numberOfArticles
    const articles = nextPage ? articleList.slice(0, -1) : articleList
    return { nextPage, articles, pageNo }
  },
  computed: {
    prevLink() {
      return this.pageNo === 2 ? '/' : `/article/page/${this.pageNo - 1}`
    },
  },
}
</script>
