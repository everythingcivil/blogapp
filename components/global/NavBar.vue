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
              data-type='[ "Blog", "Engineering", "Structure"]'
            >
              <span class="wrap"></span>
            </span>
          </NuxtLink>
          <a
            role="button"
            class="navbar-burger"
            aria-label="menu"
            aria-expanded="false"
            @click="active = !active"
          >
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
          </a>
        </div>

        <div
          :class="{
            'navbar-menu': true,
            'is-active': active,
          }"
        >
          <div class="navbar-end">
            <div
              @mouseover="toggleDropdown(true)"
              @mouseleave="toggleDropdown(false)"
              class="has-dropdown is-hoverable navbar-item"
            >
              <a class="navbar-link"> Categories </a>
              <div v-if="showDropdown" class="navbar-dropdown">
                <li
                  @click="active = false"
                  v-for="item of category"
                  :key="item.slug"
                  class="navbar-item"
                >
                  <NuxtLink :to="/categories/ + item.slug">
                    {{ item.name }}
                  </NuxtLink>
                </li>
              </div>
            </div>
            <li class="navbar-item" @click="active = false">
              <NuxtLink to="/article/popular"> Popular Articles </NuxtLink>
            </li>
            <div class="buttons search-button navbar-item">
              <p class="control" @click="active = false">
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

    <SearchBarModal v-if="showSearchModal" @close="showSearchModal = false" />
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
      active: false,
      showSearchModal: false,
      showDropdown: false,
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
    handleScroll() {
      const header = document.querySelector('.navbar')
      if (window.scrollY > 5) {
        header.classList.add('fixed')
      } else if (window.scrollY < 5) {
        header.classList.remove('fixed')
      }
    },
    toggleDropdown(payload) {
      if (window.innerWidth > 1023) {
        if (this.showDropdown !== payload) this.routeChange = false
        if (!this.routeChange) {
          this.showDropdown = payload
        }
      } else {
        this.showDropdown = true
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
    },
  },
}
</script>

<style>
body {
  font-family: Inter, -apple-system, BlinkMacSystemFont, Segoe UI, Helvetica,
    Arial, sans-serif, Apple Color Emoji, Segoe UI Emoji, Segoe UI Symbol;
}
.search-button a {
  background-color: #f5f5f5;
}
.navbar-end li a {
  color: #4a4a4a;
}
.navbar-end li:hover {
  background-color: #fafafa;
  color: #3273dc;
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
  font-family: 'Varela Round', sans-serif;
  font-style: bold;
  letter-spacing: 1px;
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
.panel-heading {
  font-weight: 600;
}
</style>
