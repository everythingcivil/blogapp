<template>
  <div>
    <div>
      <input
        v-model="searchQuery"
        type="search"
        autocomplete="off"
        placeholder="Search Articles"
      />
      <ul v-if="Sarticles.length">
        <li v-for="article of Sarticles" :key="article.slug">
          <NuxtLink :to="article.slug">
            {{ article.title }}
          </NuxtLink>
        </li>
      </ul>
    </div>
    <div>
      <h1>Categories</h1>
      <li v-for="item of category" :key="item.slug">
        <NuxtLink :to="/categories/ + item.slug">{{ item.name }}</NuxtLink>
      </li>
    </div>

    <div>
      <h1>Posts</h1>
      <li v-for="item of articles" :key="item.slug">
        <NuxtLink :to="/article/ + item.slug">{{ item.title }}</NuxtLink>
      </li>
      <section v-if="nextPage">
        <nuxt-link to="/article/page/2"> Next page </nuxt-link>
      </section>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ $content }) {
    const numberOfArticles = 5
    const articleList = await $content('articles')
      .only(['title', 'date', 'description', 'featureImage', 'slug'])
      .sortBy('date', 'desc')
      .limit(numberOfArticles)
      .fetch()
    const category = await $content('categories')
      .only(['name', 'slug'])
      .sortBy('ranking', 'asc')
      .fetch()
    const nextPage = articleList.length === numberOfArticles
    const articles = nextPage ? articleList.slice(0, -1) : articleList
    return { nextPage, articles, category }
  },
  data() {
    return {
      searchQuery: '',
      Sarticles: [],
    }
  },
  watch: {
    async searchQuery(searchQuery) {
      if (!searchQuery) {
        this.Sarticles = []
        return
      }
      this.Sarticles = await this.$content('articles')
        .limit(6)
        .search(searchQuery)
        .fetch()
    },
  },
}
</script>
