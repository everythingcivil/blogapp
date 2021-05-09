<template>
  <div>
    <nav
      class="navbar is-fixed-top has-border-bottom-gray-light py-1"
      role="navigation"
      aria-label="main navigation"
      style="transition: 0.5s"
    >
      <div class="container">
        <div class="navbar-brand">
          <NuxtLink class="navbar-item" to="/">
            Everything Civil&nbsp;
            <span
              class="typewrite"
              data-period="2000"
              data-type='[ "Blog", "Structure", "Building", "Design" ]'
            >
              <span class="wrap"></span>
            </span>
          </NuxtLink>
          <a
            :aria-expanded="isActive"
            :class="{ 'is-active': isActive }"
            role="button"
            class="navbar-burger"
            aria-label="menu"
            @click="isActive = !isActive"
          >
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
          </a>
        </div>

        <div :class="{ 'is-active': isActive }" class="navbar-menu">
          <div class="navbar-end">
            <div
              @mouseover="toggleDropdown(true)"
              @mouseleave="toggleDropdown(false)"
              class="navbar-item has-dropdown is-hoverable"
            >
              <a class="navbar-link"> Categories </a>
              <div v-if="showDropdown" class="navbar-dropdown">
                <NuxtLink
                  v-for="item of category"
                  :key="item.slug"
                  :to="/categories/ + item.slug"
                  class="navbar-item"
                >
                  {{ item.name }}
                </NuxtLink>
              </div>
            </div>
            <NuxtLink class="navbar-item" to="/article/popular">
              Popular Articles
            </NuxtLink>
            <div class="buttons search-button navbar-item">
              <p class="control">
                <a class="button" @click="showSearchModal = true">
                  <span class="icon">
                    <SearchIcon />
                  </span>
                  <span class="has-text-grey-light has-text-weight-light">
                    Search articles
                  </span>
                </a>
              </p>
            </div>
          </div>
        </div>
      </div>
    </nav>
    <SearchBarModal
      v-if="showSearchModal"
      @close="showSearchModal = false"
      @mouseover="toggleModal(true)"
      @mouseleave="toggleModal(false)"
    />
  </div>
</template>

<script>
import SearchIcon from '@/components/svg/SearchIcon'
import SearchBarModal from '@/components/articles/SearchBarModal'

export default {
  name: 'NavBar',
  components: {
    SearchIcon,
    SearchBarModal,
  },
  head: {
    htmlAttrs: {
      class: 'has-navbar-fixed-top',
    },
  },
  data() {
    return {
      isActive: false,
      showNavbar: true,
      routeChange: false,
      showDropdown: false,
      showSearchModal: false,
      category: [],
    }
  },
  beforeMount() {
    window.addEventListener('scroll', this.handleScroll)
  },
  async mounted() {
    this.category = await this.fetchCategory()
    document.addEventListener('DOMContentLoaded', () => {
      const $navbarBurgers = Array.prototype.slice.call(
        document.querySelectorAll('.navbar-burger'),
        0
      )
      if ($navbarBurgers.length > 0) {
        $navbarBurgers.forEach((el) => {
          el.addEventListener('click', () => {
            const target = el.dataset.target
            const $target = document.getElementById(target)
            el.classList.toggle('is-active')
            $target.classList.toggle('is-active')
          })
        })
      }
    })

    const TxtType = function (el, toRotate, period) {
      this.toRotate = toRotate
      this.el = el
      this.loopNum = 0
      this.period = parseInt(period, 10) || 2000
      this.txt = ''
      this.tick()
      this.isDeleting = false
    }

    TxtType.prototype.tick = function () {
      const i = this.loopNum % this.toRotate.length
      const fullTxt = this.toRotate[i]

      if (this.isDeleting) {
        this.txt = fullTxt.substring(0, this.txt.length - 1)
      } else {
        this.txt = fullTxt.substring(0, this.txt.length + 1)
      }

      this.el.innerHTML = '<span class="wrap">' + this.txt + '</span>'

      const that = this
      let delta = 200 - Math.random() * 100

      if (this.isDeleting) {
        delta /= 2
      }

      if (!this.isDeleting && this.txt === fullTxt) {
        delta = this.period
        this.isDeleting = true
      } else if (this.isDeleting && this.txt === '') {
        this.isDeleting = false
        this.loopNum++
        delta = 500
      }

      setTimeout(function () {
        that.tick()
      }, delta)
    }

    const elements = document.getElementsByClassName('typewrite')
    for (let i = 0; i < elements.length; i++) {
      const toRotate = elements[i].getAttribute('data-type')
      const period = elements[i].getAttribute('data-period')
      if (toRotate) {
        new TxtType(elements[i], JSON.parse(toRotate), period)
      }
    }
    // INJECT CSS
    const css = document.createElement('style')
    css.type = 'text/css'
    css.innerHTML = '.typewrite > .wrap { border-right: 0.13em solid #3273dc}'
    document.body.appendChild(css)
  },
  methods: {
    toggleDropdown(payload) {
      if (this.showDropdown !== payload) this.routeChange = false
      if (!this.routeChange) {
        this.showDropdown = payload
      }
    },
    toggleModal(payload) {
      if (this.showSearchModal !== payload) this.routeChange = false
      if (!this.routeChange) {
        this.showSearchModal = payload
      }
    },
    handleScroll() {
      let header = document.querySelector('.navbar')
      if (window.scrollY > 5) {
        header.classList.add('fixed')
      } else if (window.scrollY < 5) {
        header.classList.remove('fixed')
      }
    },
    async fetchCategory() {
      return await this.$content('categories')
        .only(['name', 'slug'])
        .sortBy('ranking', 'asc')
        .fetch()
    },
  },
  watch: {
    $route() {
      this.routeChange = true
      this.showDropdown = false
      this.showSearchModal = false
    },
  },
}
</script>

<style>
/* body {
  background-color: #fafafa;
} */
.search-button a {
  background-color: #f5f5f5;
}
.search-button .button:hover {
  background: #fff;
}
.search-button .button:hover .has-text-grey-light {
  color: #4a4a4a !important;
}
.typewrite {
  color: #3273dc;
}

.has-border-bottom-gray-light {
  border-bottom: 1px solid #fafafa;
}

.fixed {
  transition: 0.15s;
  box-shadow: 0 15px 50px 0 rgb(28 15 125 / 5%);
}

.navbar-brand {
  font-weight: 700;
  font-size: 1.3rem;
  font-family: VisbyRoundCF, sans-serif;
  font-style: bold;
}

.navbar-brand a:hover {
  color: inherit;
}

.navbar-brand a:focus {
  color: #4a4a4a;
}

.navbar-menu {
  font-weight: 500;
}

.container {
  padding-left: 1.5em;
  padding-right: 1.5em;
}

.media-content {
  overflow-x: initial !important;
}
.navbar-menu .NuxtLink-active {
  color: #3273dc;
  font-weight: 600;
}
@media screen and (max-width: 768px) {
  .section {
    padding: 3em 0;
  }
}
</style>
