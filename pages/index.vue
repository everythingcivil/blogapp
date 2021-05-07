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
    const articles = await $content('blog').fetch()

    return {
      articles,
    }
  },
}
</script>
