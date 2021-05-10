<template>
  <div>
    <section class="section">
      <div class="container">
        <div class="columns is-multiline">
          <div class="column is-8">
            <PanelHeading :panel-details="panelDetails" />
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
import PanelHeading from '@/components/articles/PanelHeading'

export default {
  components: {
    ArticleCard,
    LatestPanel,
    PanelHeading,
  },
  async asyncData({ $content }) {
    const articles = await $content('articles')
      .where({ isPopular: true })
      .only(['title', 'date', 'description', 'featureImage', 'slug'])
      .sortBy('date', 'desc')
      .fetch()
    return { articles }
  },
  data() {
    return {
      panelDetails: {
        name: 'Popular Articles',
        description:
          'JavaScript variables can be objects. Arrays are special kinds of objects. Because of this, you can have variables of different types in the same Array. You can have objects in an Array. You can have functions in an Array. You can have arrays in an Array:',
      },
    }
  },
}
</script>
