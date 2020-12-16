<template>
  <div class="article-page">
    <div class="banner">
      <div class="container">
        <h1>{{ article.title }}</h1>
        <article-meta :article="article" />
      </div>
    </div>

    <div class="container page">
      <div class="row article-content">
        <div class="col-md-12">
          <p>
            <client-only>
              <vue-markdown>{{ article.body }}</vue-markdown>
            </client-only>
          </p>
        </div>
      </div>

      <ul class="tag-list">
        <li v-for="tag in article.tagList" :key="tag" class="tag-default tag-pill tag-outline">
          {{ tag }}
        </li>
      </ul>

      <hr>

      <div class="article-actions">
        <article-meta :article="article" />
      </div>

      <div class="row">
        <div class="col-xs-12 col-md-8 offset-md-2">
          <div v-if="isAuthenticated">
            <errors :errors="errors" />
            <form class="card comment-form" @submit.prevent="addComment()">
              <div class="card-block">
                <textarea v-model="commentBody" class="form-control" placeholder="Write a comment..." rows="3" />
              </div>
              <div class="card-footer">
                <img ng-src="" class="comment-author-img">
                <button class="btn btn-sm btn-primary" type="submit">
                  Post Comment
                </button>
              </div>
            </form>
          </div>
          <p v-else>
            <nuxt-link to="/login">
              Sign in
            </nuxt-link> or <nuxt-link to="/register">
              Sign up
            </nuxt-link> to add comments on this article.
          </p>
          <comment v-for="comment in comments" :key="comment.id" :comment="comment" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
export default {
  async asyncData ({ params, $axios }) {
    const slug = params.slug

    const article = (await $axios.$get(`/articles/${slug}`)).article
    const comments = (await $axios.$get(`/articles/${slug}/comments`)).comments

    return { article, comments }
  },
  data () {
    return {
      commentBody: '',
      errors: null
    }
  },
  computed: {
    ...mapGetters(['isAuthenticated', 'loggedInUser'])
  },
  methods: {
    async addComment () {
      try {
        const comment = (await this.$axios.$post(`/articles/${this.article.slug}/comments`, {
          comment: {
            body: this.commentBody
          }
        })).comment

        this.comments.push(comment)
      } catch (e) {
        this.errors = e.response.data.errors
      }
    }
  }
}
</script>
