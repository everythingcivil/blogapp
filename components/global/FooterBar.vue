<template>
  <footer class="footer py-6">
    <div class="content has-text-centered">
      <div>
        <ul class="social m-0 mb-4">
          <li>
            <a href="">
              <span class="icon">
                <FacebookIcon />
              </span>
            </a>
          </li>
          <li>
            <a href="">
              <span class="icon">
                <LinkedinIcon />
              </span>
            </a>
          </li>
        </ul>
      </div>
      <p class="mb-1">
        Read the Everything Civil <a href="">Privacy Policy</a>, and
        <a href="">Terms of Service</a>.
      </p>
      <p>© 2021 Everything Civil Blog.</p>
    </div>
  </footer>
</template>

<script>
import FacebookIcon from '@/components/svg/FacebookIcon'
import LinkedinIcon from '@/components/svg/LinkedinIcon'
export default {
  name: 'Footer',
  components: {
    FacebookIcon,
    LinkedinIcon,
  },
}
</script>

<style scoped>
footer {
  background-color: #fafafa;
  border-top: 4px solid #f0f0f3;
  font-weight: 500;
}

.social li {
  display: inline;
  padding: 0.5em;
}

.icon {
  font-size: 1.6rem;
}
</style>
