<template>
  <div>
    <div class="modal is-active">
      <div class="modal-background" @click="$emit('close')"></div>
      <div id="modal-card1" class="modal-card mt-6">
        <section class="modal-card-body">
          <div class="field">
            <p class="control has-icons-left">
              <input
                v-model="searchQuery"
                class="input is-medium"
                type="search"
                autocomplete="off"
                placeholder="Search Articles"
              />
              <span class="icon is-medium is-left">
                <SearchIcon />
              </span>
            </p>
          </div>
          <div>
            <div v-if="loading" class="loading-page">
              <p>Loading...</p>
            </div>
            <ul v-if="articles.length">
              <li v-for="article of articles" :key="article.slug" class="py-1">
                <NuxtLink :to="/article/ + article.slug">
                  <div class="card">
                    <header class="card-header">
                      <p class="is-size-6 p-3 has-text-weight-medium">
                        {{ article.title }}
                      </p>
                    </header>
                  </div>
                </NuxtLink>
              </li>
            </ul>
            <ul v-else>
              <li>
                <div class="has-text-centered py-5">
                  <span class="is-size-6 has-text-weight-light is-light">
                    No result found/ No recent searches
                  </span>
                </div>
              </li>
            </ul>
          </div>
        </section>
        <footer class="modal-card-foot">
          <button class="button" @click="$emit('close')">Close search</button>
        </footer>
      </div>
    </div>
  </div>
</template>

<script>
import SearchIcon from '@/components/svg/SearchIcon'

export default {
  name: 'SearchBarModal',
  components: {
    SearchIcon,
  },
  data() {
    return {
      searchQuery: '',
      articles: [],
      loading: false,
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
  methods: {
    start() {
      this.loading = true
    },
    finish() {
      this.loading = false
    },
  },
}
</script>

<style scoped>
.modal-background {
  background-color: rgba(101, 108, 133, 0.8);
}
.modal {
  justify-content: flex-start;
}
.modal-card-body {
  background-color: #f5f6f7;
  border-top-left-radius: 6px;
  border-top-right-radius: 6px;
}
.card:hover {
  background-color: #f2f6fc;
}
.modal-card-foot {
  background-color: #fff;
  box-shadow: 0 -1px 0 0 #e0e3e8, 0 -3px 6px 0 rgba(69, 98, 155, 0.12);
}
</style>
