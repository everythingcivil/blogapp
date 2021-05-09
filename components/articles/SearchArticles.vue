<template>
  <div>
    <div class="field">
      <p class="control has-icons-left has-icons-right">
        <input
          v-model="searchQuery"
          class="input"
          type="search"
          autocomplete="off"
          placeholder="Search Articles"
        />
        <span class="icon is-small is-left">
          <SearchIcon />
        </span>
      </p>
    </div>
    <ul v-if="articles.length">
      <li v-for="article of articles" :key="article.slug">
        <NuxtLink :to="article.slug">
          {{ article.title }}
        </NuxtLink>
      </li>
    </ul>
  </div>
</template>

<script>
import SearchIcon from '@/components/svg/SearchIcon'

export default {
  name: 'SearchArticles',
  components: {
    SearchIcon,
  },
  data() {
    return {
      searchQuery: '',
      articles: [],
    }
  },
  watch: {
    async searchQuery(searchQuery) {
      if (!searchQuery) {
        this.articles = []
        return
      }
      this.articles = await this.$content('articles')
        .limit(6)
        .search(searchQuery)
        .fetch()
    },
  },
}
</script>
