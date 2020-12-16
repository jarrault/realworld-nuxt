<template>
  <div class="editor-page">
    <div class="container page">
      <div class="row">
        <div class="col-md-10 offset-md-1 col-xs-12">
          <errors :errors="errors" />
          <form>
            <fieldset>
              <fieldset class="form-group">
                <input v-model="article.title" type="text" class="form-control form-control-lg" placeholder="Article Title">
              </fieldset>
              <fieldset class="form-group">
                <input v-model="article.description" type="text" class="form-control" placeholder="What's this article about?">
              </fieldset>
              <fieldset class="form-group">
                <textarea v-model="article.body" class="form-control" rows="8" placeholder="Write your article (in markdown)" />
              </fieldset>
              <fieldset class="form-group">
                <input v-model="currentTag" type="text" class="form-control" placeholder="Enter tags" @keyup.enter="addTag(currentTag)">
                <div class="tag-list">
                  <span v-for="tag in article.tagList" :key="tag" class="tag-default tag-pill">
                    <i class="ion-close-round" @click="removeTag(tag)" />
                    {{ tag }}
                  </span>
                </div>
              </fieldset>

              <button class="btn btn-lg pull-xs-right btn-primary" type="button" @click="updateArticle()">
                Publish Article
              </button>
            </fieldset>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData ({ params, $axios }) {
    const slug = params.slug

    const article = (await $axios.$get(`/articles/${slug}`)).article

    return { article }
  },
  data () {
    return {
      currentTag: '',
      errors: null
    }
  },
  methods: {
    addTag (tag) {
      this.article.tagList.push(tag)
      this.currentTag = ''
    },
    removeTag (tag) {
      this.article.tagList.pop(tag)
    },
    async updateArticle () {
      try {
        const article = (await this.$axios.put(`/articles/${this.article.slug}`, {
          article: this.article
        })).data.article

        this.$router.push(`/article/${article.slug}`)
      } catch (e) {
        this.errors = e.response.data.errors
      }
    }
  }
}
</script>
