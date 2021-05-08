<template>
  <div>
    <div>
      <h1>{{ categories.name }}</h1>
      <li v-for="item of articles" :key="item.slug">
        <NuxtLink :to="/article/ + item.slug">{{ item.title }}</NuxtLink>
        [ {{ item.category[0] }} ]
      </li>
    </div>
  </div>
</template>

<script>
export default {
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
