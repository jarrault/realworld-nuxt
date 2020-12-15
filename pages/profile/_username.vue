<template>
  <div class="profile-page">
    <div class="user-info">
      <div class="container">
        <div class="row">
          <div class="col-xs-12 col-md-10 offset-md-1">
            <img :src="user.image" class="user-img">
            <h4>{{ user.username }}</h4>
            <p>
              {{ user.bio }}
            </p>
            <nuxt-link v-if="isCurrentUser" class="btn btn-sm btn-outline-secondary action-btn" to="/settings">
              <i class="ion-gear-a" /> Edit Profile Settings
            </nuxt-link>
            <button v-else class="btn btn-sm btn-outline-secondary action-btn">
              <i class="ion-plus-round" />
              Follow {{ user.username }}
            </button>
          </div>
        </div>
      </div>
    </div>

    <div class="container">
      <div class="row">
        <div class="col-xs-12 col-md-10 offset-md-1">
          <div class="articles-toggle">
            <ul class="nav nav-pills outline-active">
              <li class="nav-item">
                <a class="nav-link" :class="{'active' : !favorited}" href @click.prevent="updateArticles()">My Articles</a>
              </li>
              <li class="nav-item">
                <a class="nav-link" :class="{'active' : favorited}" href @click.prevent="updateArticles()">
                  Favorited Articles</a>
                </a>
              </li>
            </ul>
          </div>

          <article-preview v-for="article in articles" :key="article.slug" :article="article" />

          <div v-if="!articles.length" class="article-preview">
            No articles are here... yet.
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  async asyncData ({ params, $axios }) {
    const username = params.username
    const user = (await $axios.$get(`/profiles/${username}`)).profile
    const articles = (await $axios.$get(`/articles?author=${username}&limit=5&offset=0`)).articles

    return { user, articles }
  },
  data () {
    return {
      favorited: false,
      articlesCount: 0,
      page: 1
    }
  },
  computed: {
    ...mapGetters(['isAuthenticated', 'loggedInUser']),
    isCurrentUser () {
      if (!this.isAuthenticated) {
        return false
      }

      return this.user.username === this.loggedInUser.username
    }
  },
  methods: {
    async updateArticles () {
      let type = 'author'

      if (!this.favorited) {
        type = 'favorited'
        this.favorited = true
      } else {
        this.favorited = false
      }

      this.articles = (await this.$axios.$get(`/articles?${type}=${this.user.username}&limit=5&offset=0`)).articles
    }
  }
}
</script>
