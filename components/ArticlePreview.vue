<template>
  <div class="article-preview">
    <div class="article-meta">
      <a href=""><img :src="article.author.image"></a>
      <div class="info">
        <a href="" class="author">{{ article.author.username }}</a>
        <span class="date">{{ article.createdAt | dateParse('YYYY-MM-DD HH:mm:ss') | dateFormat('MMMM D, YYYY') }}</span>
      </div>
      <button class="btn btn-outline-primary btn-sm pull-xs-right">
        <i class="ion-heart" /> {{ article.favoritesCount }}
      </button>
    </div>
    <nuxt-link :to="{ name: 'article-slug', params: { slug: article.slug }}" class="preview-link">
      <h1>{{ article.title }}</h1>
      <p>{{ article.description }}</p>
      <span>Read more...</span>
      <ul class="tag-list">
        <li v-for="tag in article.tagList" :key="tag" class="tag-default tag-pill tag-outline">
          {{ tag }}
        </li>
      </ul>
    </nuxt-link>
  </div>
</template>

<script>
export default {
  props: {
    article: {
      type: Object,
      default: null
    }
  }
}
</script>
