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
          <p><a href="#/login">Sign in</a> or <a ui-sref="app.register" href="#/register">sign up</a> to add comments on this article.</p>
          <!-- <form class="card comment-form">
            <div class="card-block">
              <textarea class="form-control" placeholder="Write a comment..." rows="3" />
            </div>
            <div class="card-footer">
              <img src="http://i.imgur.com/Qr71crq.jpg" class="comment-author-img">
              <button class="btn btn-sm btn-primary">
                Post Comment
              </button>
            </div>
          </form> -->
          <comment v-for="comment in comments" :key="comment.id" :comment="comment" />
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
    const comments = (await $axios.$get(`/articles/${slug}/comments`)).comments
    return { article, comments }
  }
}
</script>
