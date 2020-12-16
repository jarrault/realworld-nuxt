<template>
  <div class="editor-page">
    <div class="container page">
      <div class="row">
        <div class="col-md-10 offset-md-1 col-xs-12">
          <errors :errors="errors" />
          <form>
            <fieldset>
              <fieldset class="form-group">
                <input v-model="title" type="text" class="form-control form-control-lg" placeholder="Article Title">
              </fieldset>
              <fieldset class="form-group">
                <input v-model="description" type="text" class="form-control" placeholder="What's this article about?">
              </fieldset>
              <fieldset class="form-group">
                <textarea v-model="body" class="form-control" rows="8" placeholder="Write your article (in markdown)" />
              </fieldset>
              <fieldset class="form-group">
                <input v-model="currentTag" type="text" class="form-control" placeholder="Enter tags" @keyup.enter="addTag(currentTag)">
                <div class="tag-list">
                  <span v-for="tag in tagList" :key="tag" class="tag-default tag-pill">
                    <i class="ion-close-round" @click="removeTag(tag)" />
                    {{ tag }}
                  </span>
                </div>
              </fieldset>

              <button class="btn btn-lg pull-xs-right btn-primary" type="button" @click="createArticle()">
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
  data () {
    return {
      title: '',
      body: '',
      description: '',
      tagList: [],
      currentTag: '',
      errors: null
    }
  },
  methods: {
    addTag (tag) {
      this.tagList.push(tag)
      this.currentTag = ''
    },
    removeTag (tag) {
      this.tagList.pop(tag)
    },
    async createArticle () {
      try {
        const article = (await this.$axios.$post('/articles', {
          article: {
            title: this.title,
            body: this.body,
            description: this.description,
            tagList: this.tagList
          }
        })).article

        this.$router.push(`/article/${article.slug}`)
      } catch (e) {
        this.errors = e.response.data.errors
      }
    }
  }
}
</script>
