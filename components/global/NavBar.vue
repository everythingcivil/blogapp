<template>
  <nav
    class="navbar is-fixed-top has-border-bottom-gray-light py-1"
    role="navigation"
    aria-label="main navigation"
    style="transition: 0.5s"
  >
    <div class="container">
      <div class="navbar-brand">
        <NuxtLink class="navbar-item" to="/">
          Everything&nbsp;
          <span
            class="typewrite"
            data-period="2000"
            data-type='[ "Civil Blog", "Building", "Design" ]'
          >
            <span class="wrap"></span>
          </span>
        </NuxtLink>

        <a
          role="button"
          class="navbar-burger"
          aria-label="menu"
          aria-expanded="false"
          data-target="navbarToggle"
        >
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
        </a>
      </div>

      <div id="navbarToggle" class="navbar-menu">
        <div class="navbar-end">
          <div class="navbar-item has-dropdown is-hoverable">
            <a class="navbar-link"> Blog </a>
            <div class="navbar-dropdown">
              <a class="navbar-item"> Structural Engineering </a>
              <a class="navbar-item"> Structural Design </a>
              <a class="navbar-item"> Building Materials </a>
            </div>
          </div>
          <a href="" class="navbar-item">Popular Articles</a>
          <NuxtLink class="navbar-item" to="/about"> About Us </NuxtLink>
        </div>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'NavBar',
  head: {
    htmlAttrs: {
      class: 'has-navbar-fixed-top',
    },
  },
  beforeMount() {
    window.addEventListener('scroll', this.handleScroll)
  },
  mounted() {
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

    var TxtType = function (el, toRotate, period) {
      this.toRotate = toRotate
      this.el = el
      this.loopNum = 0
      this.period = parseInt(period, 10) || 2000
      this.txt = ''
      this.tick()
      this.isDeleting = false
    }

    TxtType.prototype.tick = function () {
      var i = this.loopNum % this.toRotate.length
      var fullTxt = this.toRotate[i]

      if (this.isDeleting) {
        this.txt = fullTxt.substring(0, this.txt.length - 1)
      } else {
        this.txt = fullTxt.substring(0, this.txt.length + 1)
      }

      this.el.innerHTML = '<span class="wrap">' + this.txt + '</span>'

      var that = this
      var delta = 200 - Math.random() * 100

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

    var elements = document.getElementsByClassName('typewrite')
    for (var i = 0; i < elements.length; i++) {
      var toRotate = elements[i].getAttribute('data-type')
      var period = elements[i].getAttribute('data-period')
      if (toRotate) {
        new TxtType(elements[i], JSON.parse(toRotate), period)
      }
    }
    // INJECT CSS
    var css = document.createElement('style')
    css.type = 'text/css'
    css.innerHTML = '.typewrite > .wrap { border-right: 0.13em solid #3273dc}'
    document.body.appendChild(css)
  },
  methods: {
    handleScroll() {
      let header = document.querySelector('.navbar')
      if (window.scrollY > 5) {
        header.classList.add('fixed')
      } else if (window.scrollY < 5) {
        header.classList.remove('fixed')
      }
    },
  },
}
</script>

<style>
body {
  font-family: 'Open Sans', sans-serif !important;
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
</style>
