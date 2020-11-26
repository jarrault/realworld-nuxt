<template>
  <nav class="navbar navbar-light">
    <div class="container">
      <nuxt-link class="navbar-brand" to="/">
        conduit
      </nuxt-link>
      <ul v-if="isAuthenticated" class="nav navbar-nav pull-xs-right">
        <li class="nav-item">
          <nuxt-link class="nav-link" to="/">
            Home
          </nuxt-link>
        </li>
        <li class="nav-item">
          <nuxt-link class="nav-link" to="">
            <i class="ion-compose" />&nbsp;New Article
          </nuxt-link>
        </li>
        <li class="nav-item">
          <nuxt-link class="nav-link" to="/settings">
            <i class="ion-gear-a" />&nbsp;Settings
          </nuxt-link>
        </li>
        <li class="nav-item">
          <nuxt-link class="nav-link" to="">
            {{ loggedInUser.username }}
          </nuxt-link>
        </li>
      </ul>
      <ul v-else class="nav navbar-nav pull-xs-right">
        <li class="nav-item">
          <nuxt-link class="nav-link" to="/">
            Home
          </nuxt-link>
        </li>
        <li class="nav-item">
          <nuxt-link class="nav-link" to="/login">
            Sign in
          </nuxt-link>
        </li>
        <li class="nav-item">
          <nuxt-link class="nav-link" to="/register">
            Sign up
          </nuxt-link>
        </li>
      </ul>
    </div>
  </nav>
</template>

<script>
import { mapGetters } from 'vuex'
export default {
  computed: {
    ...mapGetters(['isAuthenticated', 'loggedInUser'])
  }
}
</script>
