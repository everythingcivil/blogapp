<template>
  <div>
    <section class="section">
      <div class="container">
        <div class="columns is-multiline">
          <div class="column is-8">
            <PanelHeading :panel-details="categories" />
            <ArticleCard :articles="articles" />
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
import PanelHeading from '@/components/articles/PanelHeading'

export default {
  components: {
    PopularPanel,
    ArticleCard,
    PanelHeading,
  },
  async asyncData({ $content, params }) {
    const categories = await $content('categories', params.slug).fetch()
    const articles = await $content('articles')
      .where({ category: { $contains: categories.name } })
      .fetch()
    return {
      categories,
      articles,
    }
  },
}
</script>
