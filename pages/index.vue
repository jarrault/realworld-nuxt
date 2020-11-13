<template>
  <div class="home-page">
    <div class="banner">
      <div class="container">
        <h1 class="logo-font">
          conduit
        </h1>
        <p>A place to share your knowledge.</p>
      </div>
    </div>

    <div class="container page">
      <div class="row">
        <div class="col-md-9">
          <div class="feed-toggle">
            <ul class="nav nav-pills outline-active">
              <li class="nav-item">
                <a class="nav-link disabled" href="">Your Feed</a>
              </li>
              <li class="nav-item">
                <a class="nav-link active" href="">Global Feed</a>
              </li>
            </ul>
          </div>

          <article-preview v-for="article in articles" :key="article.slug" :article="article" />
        </div>

        <div class="col-md-3">
          <div class="sidebar">
            <p>Popular Tags</p>

            <div class="tag-list">
              <a v-for="tag in tags" :key="tag" href="/#/" class="tag-pill tag-default" @click="getArticleByTag(tag)">{{ tag }}</a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData ({ $axios }) {
    const tags = (await $axios.$get('/tags')).tags
    const articles = (await $axios.$get('/articles')).articles
    return { tags, articles }
  },
  data () {
    return {
      articles: []
    }
  },
  methods: {
    async getArticleByTag (tag) {
      this.articles = (await this.$axios.$get(`/articles?tag=${tag}`)).articles
    }
  }
}
</script>
