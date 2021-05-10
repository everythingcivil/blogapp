<template>
  <div class="column is-4">
    <article class="panel is-link">
      <p class="panel-heading py-4">Latest Articles</p>
      <div v-for="item in popularArticles" :key="item.slug" class="panel-block">
        <NuxtLink :to="/article/ + item.slug" class="control">
          <div class="x-small">
            {{ getFormattedDate(item.date) }}
          </div>
          <div class="small">
            {{ item.title }}
          </div>
        </NuxtLink>
      </div>
      <div class="panel-block">
        <NuxtLink to="/" class="button is-link is-outlined is-fullwidth">
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
    getFormattedDate(dateString) {
      const date = new Date(dateString)
      const options = {
        weekday: 'long',
        year: 'numeric',
        month: 'long',
        day: 'numeric',
      }
      return date
        .toLocaleDateString('en-US', options)
        .split(' ')
        .slice(1)
        .join(' ')
    },
    async fetchPosts() {
      return await this.$content('articles')
        .only(['title', 'date', 'slug'])
        .sortBy('date', 'desc')
        .limit(6)
        .fetch()
    },
  },
}
</script>

<style scoped>
.small {
  font-size: 0.85rem;
  font-weight: 500;
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
.panel-block:hover .small {
  color: #3273dc;
}
.panel-block a {
  transition: 0.15s;
}
.panel-heading {
  font-size: 1rem;
  font-weight: 600;
}
</style>
