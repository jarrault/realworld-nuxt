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
    <button v-else class="btn btn-sm btn-outline-secondary">
      <i class="ion-plus-round" />
      &nbsp; Follow {{ article.author.username }}
      <span v-if="article.author.following" class="counter">({{ article.author.following }})</span>
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
    }
  }
}
</script>
