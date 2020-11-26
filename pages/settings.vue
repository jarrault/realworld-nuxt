<template>
  <div class="settings-page">
    <div class="container page">
      <div class="row">
        <div class="col-md-6 offset-md-3 col-xs-12">
          <h1 class="text-xs-center">
            Your Settings
          </h1>

          <errors :errors="errors" />

          <form @submit.prevent="update">
            <fieldset>
              <fieldset class="form-group">
                <input
                  v-model="currentUser.image"
                  class="form-control"
                  type="text"
                  placeholder="URL of profile picture"
                >
              </fieldset>
              <fieldset class="form-group">
                <input
                  v-model="currentUser.username"
                  class="form-control form-control-lg"
                  type="text"
                  placeholder="Your Name"
                >
              </fieldset>
              <fieldset class="form-group">
                <textarea
                  v-model="currentUser.bio"
                  class="form-control form-control-lg"
                  rows="8"
                  placeholder="Short bio about you"
                />
              </fieldset>
              <fieldset class="form-group">
                <input
                  v-model="currentUser.email"
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
                Update Settings
              </button>
            </fieldset>
          </form>
          <hr>
          <button class="btn btn-outline-danger" @click="logout">
            Or click here to logout.
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  middleware: 'auth',
  data () {
    return {
      password: null,
      errors: null
    }
  },
  computed: {
    currentUser () {
      return {
        ...this.$auth.user
      }
    }
  },
  methods: {
    async logout () {
      await this.$auth.logout()
    },
    async update () {
      try {
        let user = {
          bio: this.currentUser.bio,
          email: this.currentUser.email,
          username: this.currentUser.username,
          image: this.currentUser.image
        }

        if (this.password) {
          user = { ...user, password: this.password }
        }

        await this.$axios.$put('/user', { user })
        this.$auth.fetchUser()
        await this.$router.push('/')
      } catch (e) {
        this.errors = e.response.data.errors
      }
    }
  }
}
</script>
