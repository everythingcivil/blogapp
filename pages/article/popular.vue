<template>
  <div>
    <section class="section">
      <div class="container">
        <div class="columns is-multiline">
          <div class="column is-8">
            <ArticleCard :articles="articles" />
          </div>
          <LatestPanel />
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import ArticleCard from '@/components/articles/ArticleCard'
import LatestPanel from '@/components/articles/LatestPanel'

export default {
  components: {
    ArticleCard,
    LatestPanel,
  },
  async asyncData({ $content }) {
    const articles = await $content('articles')
      .where({ isPopular: true })
      .only(['title', 'date', 'description', 'featureImage', 'slug'])
      .sortBy('date', 'asc')
      .fetch()
    return { articles }
  },
}
</script>
