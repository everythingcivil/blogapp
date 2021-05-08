<template>
  <div class="grid gap-4 grid-cols-4">
    <span v-if="currentPage === 1" :class="disabledStyle"> First </span>

    <nuxt-link
      v-else
      :to="{ name: 'article-page-page', params: { page: 1 } }"
      :class="buttonStyles"
    >
      First
    </nuxt-link>

    <span v-if="currentPage === 1" :class="disabledStyle"> Prev </span>

    <nuxt-link
      v-else
      :to="{ name: 'article-page-page', params: { page: prevPage } }"
      :class="buttonStyles"
    >
      Prev
    </nuxt-link>

    <span v-if="currentPage === totalPages" :class="disabledStyle"> Next </span>

    <nuxt-link
      v-else
      :to="{ name: 'article-page-page', params: { page: nextPage } }"
      :class="buttonStyles"
    >
      Next
    </nuxt-link>

    <span v-if="currentPage === totalPages" :class="disabledStyle"> Last </span>

    <nuxt-link
      v-else
      :to="{ name: 'article-page-page', params: { page: totalPages } }"
      :class="buttonStyles"
    >
      Last
    </nuxt-link>
  </div>
</template>

<script>
export default {
  props: {
    total: {
      type: Number,
      default: 0,
    },
    perPage: {
      type: Number,
      default: 5,
    },
  },
  computed: {
    totalPages() {
      return Math.floor(this.total / this.perPage)
    },
    currentPage() {
      return parseInt(this.$route.params.page) || 1
    },
    prevPage() {
      return this.currentPage > 1 ? this.currentPage - 1 : 1
    },
    nextPage() {
      return this.currentPage < this.totalPages
        ? this.currentPage + 1
        : this.totalPages
    },
  },
}
</script>
