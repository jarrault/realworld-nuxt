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
              <!-- <li class="nav-item">
                <a class="nav-link disabled" href="">Your Feed</a>
              </li> -->
              <li class="nav-item">
                <a class="nav-link active" href="">Global Feed</a>
              </li>
            </ul>
          </div>

          <article-preview v-for="article in articles" :key="article.slug" :article="article" />

          <nav>
            <paginate
              v-model="page"
              :page-count="pageCount"
              :page-range="3"
              :margin-pages="3"
              :prev-text="'Prev'"
              :next-text="'Next'"
              :container-class="'pagination'"
              :page-class="'page-item'"
              :page-link-class="'page-link'"
              :prev-class="'page-item'"
              :prev-link-class="'page-link'"
              :next-class="'page-item'"
              :next-link-class="'page-link'"
            />
          </nav>
        </div>

        <div class="col-md-3">
          <div class="sidebar">
            <p>Popular Tags</p>
            <div class="tag-list">
              <a v-for="tag in tags" :key="tag" href class="tag-pill tag-default" @click.prevent="currentTag = tag">{{ tag }}</a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const articlePerPage = 10

export default {
  async asyncData ({ $axios }) {
    const [tags, articles] = await Promise.all([
      $axios.$get('/tags'),
      $axios.$get(`/articles?limit=${articlePerPage}&offset=0`)
    ])

    return { tags: tags.tags, articles: articles.articles, articlesCount: articles.articlesCount }
  },
  data () {
    return {
      articles: [],
      articlesCount: 0,
      page: 1,
      currentTag: null
    }
  },
  computed: {
    pageCount () {
      return this.articlesCount / articlePerPage
    },
    serverParameters () {
      const query = {
        limit: articlePerPage,
        offset: (this.page - 1) * articlePerPage,
        tag: this.currentTag
      }

      Object.keys(query).forEach(key => (query[key] == null) && delete query[key])
      return query
    }
  },
  watch: {
    serverParameters: {
      deep: true,
      handler () {
        this.getArticles()
      }
    },
    currentTag () {
      this.page = 1
    }
  },
  methods: {
    async getArticles () {
      this.articles = (await this.$axios.$get('/articles', {
        params: this.serverParameters
      })).articles
    }
  }
}
</script>
