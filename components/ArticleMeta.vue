<template>
  <div class="article-meta">
    <a href=""><img :src="article.author.image"></a>
    <div class="info">
      <a href="" class="author">{{ article.author.username }}</a>
      <span class="date">{{ article.createdAt | dateParse('YYYY-MM-DD HH:mm:ss') | dateFormat('MMMM D, YYYY') }}</span>
    </div>
    <nuxt-link v-if="isAuthorOfTheArticle" class="btn btn-outline-secondary btn-sm" :to="{ name: 'editor-slug', params: { slug: article.slug }}">
      <i class="ion-edit" /> Edit Article
    </nuxt-link>
    <button v-else class="btn btn-sm btn-outline-secondary" @click="toggleFollow()">
      <i class="ion-plus-round" />
      <span v-if="!article.author.following">Follow</span> <span v-else @click="unfollowAuthor()">Unfollow</span> {{ article.author.username }}
    </button>
    &nbsp;&nbsp;
    <button v-if="isAuthorOfTheArticle" class="btn btn-outline-danger btn-sm" @click="deleteArticle()">
      <i class="ion-trash-a" /> Delete Article
    </button>
    <button v-else class="btn btn-sm btn-outline-primary">
      <i class="ion-heart" />
      &nbsp; Favorite Article
      <span class="counter">({{ article.favoritesCount }})</span>
    </button>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  props: {
    article: {
      type: Object,
      default: null
    }
  },
  computed: {
    ...mapGetters(['isAuthenticated', 'loggedInUser']),
    isAuthorOfTheArticle () {
      if (!this.isAuthenticated) {
        return false
      }

      return this.article.author.username === this.loggedInUser.username
    }
  },
  methods: {
    async deleteArticle () {
      await this.$axios.delete(`/articles/${this.article.slug}`)
      this.$router.push('/')
    },
    async toggleFollow () {
      if (!this.article.author.following) {
        await this.$axios.post(`/profiles/${this.article.author.username}/follow`)
      } else {
        await this.$axios.delete(`/profiles/${this.article.author.username}/follow`)
      }

      this.article.author.following = !this.article.author.following
    }
  }
}
</script>
