<template>
  <section class="section mb-5">
    <div
      class="article-cover"
      :style="{ backgroundImage: 'url(' + article.featureImage + ')' }"
    >
      <div class="overlay"></div>
    </div>
    <div class="container is-fluid content">
      <div class="article-margin"></div>
      <div class="article-wrapper">
        <div class="share">
          <a href="">
            <FacebookIcon />
          </a>
          <a href="">
            <LinkedinIcon />
          </a>
          <a href="">
            <PrintIcon />
          </a>
        </div>
        <header class="p-5">
          <div class="article-date caption">
            {{ formatDate(article.date) }}
          </div>
          <h1 class="h2 article-title mt-0">{{ article.title }}</h1>
          <div class="divider-little"></div>
          <p class="article-summary mt-3">
            {{ article.description }}
          </p>
        </header>
        <div class="divider"></div>
        <nuxt-content :document="article" class="p-5" />
        <div class="divider"></div>
        <div class="article-author is-flex is-align-items-center p-5">
          <div class="avatar mr-5">
            <img
              class="rounded-circle bg-gray-300"
              :src="author.image"
              :alt="author.name"
            />
          </div>
          <div class="column p-0">
            <h4 class="mb-2">{{ author.name }}</h4>
            <div style="font-size: 0.9rem">
              {{ author.description }}
            </div>
            <div class="pt-3">
              <a :href="author.linkedin" target="_blank">
                <span class="icon"><LinkedinIcon /></span>
                {{ author.name }}
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import FacebookIcon from '@/components/svg/FacebookIcon'
import LinkedinIcon from '@/components/svg/LinkedinIcon'
import PrintIcon from '@/components/svg/PrintIcon'

export default {
  name: 'ArticleView',
  components: {
    FacebookIcon,
    LinkedinIcon,
    PrintIcon,
  },
  props: {
    article: {
      type: Object,
      default: Object,
    },
    author: {
      type: Object,
      default: Object,
    },
  },
  methods: {
    formatDate(dateString) {
      const date = new Date(dateString)
      return date.toLocaleDateString() || ''
    },
  },
}
</script>
<style scoped>
.article-cover {
  background-position: 50%;
  background-size: cover;
  margin-bottom: 15px;
  width: 100%;
  height: 60vh;
  box-shadow: 0 0 1px 0 rgb(0 0 0 / 44%);
  position: absolute;
  top: 0;
  left: 0;
}

@media screen and (max-width: 768px) {
  .article-wrapper {
    top: 3rem;
    border-radius: 0;
  }
}

.article-wrapper {
  width: 100%;
  max-width: 800px;
  border-radius: 6px;
  background-color: #fff;
  border: 1px solid #d7d7d7;
  position: relative;
  margin: 0 auto;
}

.share {
  position: absolute;
  top: 2rem;
  right: 40px;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
  -ms-flex-flow: column;
  flex-flow: column;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
  font-size: 1.2em;
}

.share a {
  color: #383737;
  margin-bottom: 0.4rem;
  font-size: 0.98rem;
}

.article-date {
  text-align: left;
  max-width: 930px;
}

@media screen and (max-width: 768px) {
  .article-title {
    max-width: 75%;
  }
}

.article-margin {
  margin-top: 8em;
}

@media screen and (max-width: 1024px) {
  .article-margin {
    margin-top: 0;
  }
}

.article-title {
  display: inline-block;
  text-align: left;
  color: #241f46;
}

.content .h2,
.content h2 {
  font-size: 1.4rem;
  line-height: 2rem;
  font-weight: 600;
}

.divider-little {
  width: 4rem;
  height: 0.2rem;
  background: linear-gradient(90deg, #0967fe, #2a3ef3);
  border-radius: 20px;
}

.article-summary {
  text-align: left;
  color: #0f0428;
  max-width: 930px;
  line-height: 1.4;
  font-weight: 500;
}

.divider {
  border-bottom: 1px solid #e3e3e3;
}

.article-content {
  max-width: 930px;
  line-height: 1.5rem;
  color: #0f0428;
  font-family: 'Open Sans', sans-serif;
}

.avatar img {
  height: 6rem;
  width: 6rem;
  border: 1px solid #ccc;
}
.overlay {
  position: absolute;
  height: 100%;
  width: 100%;
  top: 0;
  left: 0;
  background: #563d7c;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  opacity: 0.4;
}
</style>
