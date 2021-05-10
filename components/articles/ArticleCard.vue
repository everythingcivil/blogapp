<template>
  <div>
    <div v-for="item in articles" :key="item.slug" class="pb-5">
      <div class="card">
        <div class="card-image">
          <NuxtLink :to="/article/ + item.slug">
            <figure class="image">
              <img :src="item.featureImage" alt="Placeholder image" />
            </figure>
          </NuxtLink>
        </div>
        <div class="card-content p-3">
          <div class="media mb-3">
            <div class="media-content">
              <p class="is-size-7 mb-2">{{ getFormattedDate(item.date) }}</p>
              <NuxtLink :to="/article/ + item.slug">
                <p class="article-title">{{ item.title }}</p>
              </NuxtLink>
            </div>
          </div>

          <div class="content">
            {{ item.description }}
          </div>
          <NuxtLink :to="/article/ + item.slug" class="button is-link">
            Read more
            <ArrowRightIcon />
          </NuxtLink>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ArrowRightIcon from '@/components/svg/ArrowRightIcon'

export default {
  components: {
    ArrowRightIcon,
  },
  props: {
    articles: {
      type: Array,
      default: Array,
    },
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
  },
}
</script>

<style scoped>
svg {
  font-size: 0.96rem;
  margin-left: 3px;
}
.card {
  box-shadow: 0 2px 2px 0 rgb(0 0 0 / 14%), 0 3px 1px -2px rgb(0 0 0 / 20%),
    0 1px 5px 0 rgb(0 0 0 / 12%);
}
.card:hover {
  box-shadow: 0 0.5em 1em -0.125em rgb(10 10 10 / 10%),
    0 0 0 1px rgb(10 10 10 / 2%);
  transition: all 0.2s linear;
}

.article-title {
  font-weight: 600;
  font-size: 1.2rem;
}

.article-title:hover {
  text-decoration: underline;
}
.card-image {
  border-bottom: 1px solid #dee2e6;
}
</style>
