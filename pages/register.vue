<template>
  <div class="auth-page">
    <div class="container page">
      <div class="row">
        <div class="col-md-6 offset-md-3 col-xs-12">
          <h1 class="text-xs-center">
            Sign up
          </h1>
          <p class="text-xs-center">
            <nuxt-link to="/login">
              Have an account?
            </nuxt-link>
          </p>

          <auth-errors :errors="errors" />

          <form @submit.prevent="register">
            <fieldset class="form-group">
              <input
                v-model="username"
                class="form-control form-control-lg"
                type="text"
                placeholder="Your Name"
              >
            </fieldset>
            <fieldset class="form-group">
              <input
                v-model="email"
                class="form-control form-control-lg"
                type="email"
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
              Sign up
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      username: '',
      email: '',
      password: '',
      errors: null
    }
  },
  methods: {
    async register () {
      try {
        await this.$axios.$post('users', {
          user: {
            email: this.email,
            username: this.username,
            password: this.password
          }
        })

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
