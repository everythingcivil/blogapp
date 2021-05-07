<template>
  <div>
    <li v-for="item of articles" :key="item.slug">
      <NuxtLink :to="/article/ + item.slug">{{ item.title }}</NuxtLink>
    </li>
  </div>
</template>

<script>
export default {
  async asyncData({ $content }) {
    const articles = await $content('articles')
      .only(['title', 'date', 'description', 'featureImage', 'slug'])
      .sortBy('date', 'desc')
      .limit(10)
      .fetch()

    return {
      articles,
    }
  },
}
</script>
