<template>
  <div>
    <section class="section">
      <div class="container">
        <div class="columns is-multiline">
          <div class="column is-8">
            <ArticleCard :articles="articles" />
            <nav class="pagination" role="navigation" aria-label="pagination">
              <NuxtLink :to="prevLink" class="pagination-previous"
                >Previous</NuxtLink
              >
              <NuxtLink
                v-if="nextPage"
                :to="`/article/page/${pageNo + 1}`"
                class="pagination-next"
                >Next page</NuxtLink
              >
            </nav>
          </div>
          <PopularPanel />
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import ArticleCard from '@/components/articles/ArticleCard'
import PopularPanel from '@/components/articles/PopularPanel'

export default {
  components: {
    ArticleCard,
    PopularPanel,
  },
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
