<template>
  <div>
    <section class="section">
      <div class="container">
        <div class="columns is-multiline">
          <div class="column is-8">
            <ArticleCard :articles="articles" />
            <nav class="pagination" role="navigation" aria-label="pagination">
              <a class="pagination-previous" disabled>Previous</a>
              <NuxtLink to="/article/page/2" class="pagination-next">
                Next page
              </NuxtLink>
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
  async asyncData({ $content }) {
    const numberOfArticles = 5
    const articleList = await $content('articles')
      .only(['title', 'date', 'description', 'featureImage', 'slug'])
      .sortBy('date', 'asc')
      .limit(numberOfArticles)
      .fetch()
    const nextPage = articleList.length === numberOfArticles
    const articles = nextPage ? articleList.slice(0, -1) : articleList
    return { nextPage, articles }
  },
}
</script>
