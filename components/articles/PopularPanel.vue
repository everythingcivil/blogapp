<template>
  <div class="column is-4">
    <article class="panel is-link">
      <p class="panel-heading py-4" style="font-size: 0.97rem">
        Popular Articles
      </p>
      <div v-for="item in popularArticles" :key="item.slug" class="panel-block">
        <NuxtLink :to="/article/ + item.slug" class="control">
          <div class="x-small">
            {{ formatDate(item.date) }}
          </div>
          <div class="small">
            {{ item.title }}
          </div>
        </NuxtLink>
      </div>
      <div class="panel-block">
        <NuxtLink
          to="/article/popular/"
          class="button is-link is-outlined is-fullwidth"
        >
          View all
        </NuxtLink>
      </div>
    </article>
  </div>
</template>

<script>
export default {
  name: 'PopularArticles',
  data() {
    return {
      popularArticles: [],
    }
  },
  async mounted() {
    this.popularArticles = await this.fetchPosts()
  },
  methods: {
    formatDate(dateString) {
      const date = new Date(dateString)
      return date.toLocaleDateString() || ''
    },
    async fetchPosts() {
      return await this.$content('articles')
        .only(['title', 'date', 'slug'])
        .where({ isPopular: true })
        .sortBy('date', 'asc')
        .limit(6)
        .fetch()
    },
  },
}
</script>

<style scoped>
.small {
  font-size: 0.83rem;
  font-weight: 600;
  color: #4a515b;
}

.x-small {
  font-size: 0.75rem;
  color: #a7aeb8;
}
.panel {
  position: sticky;
  top: 100px;
}
.panel-block:hover {
  background-color: #f2f6fc;
}
.panel-block a {
  transition: 0.15s;
}
</style>
