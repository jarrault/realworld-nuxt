<template>
  <div class="card">
    <div class="card-block">
      <p class="card-text">
        {{ comment.body }}
      </p>
    </div>
    <div class="card-footer">
      <a href class="comment-author">
        <img :src="comment.author.image" class="comment-author-img">
      </a>
      &nbsp;
      <a href class="comment-author">{{ comment.author.username }}</a>
      <span class="date-posted">{{ comment.createdAt | dateParse('YYYY-MM-DD HH:mm:ss') | dateFormat('MMMM D, YYYY') }}</span>
      <span v-if="isAuthorOfTheComment" class="mod-options">
        <i class="ion-trash-a" @click="deleteComment()" />
      </span>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  props: {
    comment: {
      type: Object,
      default: null
    },
    slug: {
      type: String,
      default: null
    }
  },
  computed: {
    ...mapGetters(['isAuthenticated', 'loggedInUser']),
    isAuthorOfTheComment () {
      if (!this.isAuthenticated) {
        return false
      }

      return this.comment.author.username === this.loggedInUser.username
    }
  },
  methods: {
    async deleteComment () {
      try {
        await this.$axios.$delete(`/articles/${this.slug}/comments/${this.comment.id}`)
        await this.$emit('delete', this.comment)
      } catch (e) {
        this.error = 'Error while deleting the comment' // Untreated error
      }
    }
  }
}
</script>
