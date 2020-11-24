<template>
  <div class="auth-page">
    <div class="container page">
      <div class="row">
        <div class="col-md-6 offset-md-3 col-xs-12">
          <h1 class="text-xs-center">
            Sign in
          </h1>
          <p class="text-xs-center">
            <nuxt-link to="/register">
              Need an account?
            </nuxt-link>
          </p>

          <auth-errors :errors="errors" />

          <form @submit.prevent="login">
            <fieldset class="form-group">
              <input
                v-model="email"
                class="form-control form-control-lg"
                type="text"
                placeholder="Email"
              >
            </fieldset>
            <fieldset class="form-group">
              <input
                v-model="password"
                class="form-control form-control-lg"
                type="password"
                placeholder="Password"
              >
            </fieldset>
            <button class="btn btn-lg btn-primary pull-xs-right">
              Sign in
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import AuthErrors from '../components/AuthErrors.vue'
export default {
  components: { AuthErrors },
  data () {
    return {
      email: '',
      password: '',
      errors: null
    }
  },
  methods: {
    async login () {
      try {
        await this.$auth.loginWith('local', {
          data: {
            user: {
              email: this.email,
              password: this.password
            }
          }
        })
      } catch (e) {
        this.errors = e.response.data.errors
      }
    }
  }
}
</script>
