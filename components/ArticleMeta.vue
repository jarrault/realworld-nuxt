<template>
  <div class="article-meta">
    <a href=""><img :src="article.author.image"></a>
    <div class="info">
      <a href="" class="author">{{ article.author.username }}</a>
      <span class="date">{{ article.createdAt | dateParse('YYYY-MM-DD HH:mm:ss') | dateFormat('MMMM D, YYYY') }}</span>
    </div>
    <button class="btn btn-sm btn-outline-secondary">
      <i class="ion-plus-round" />
      &nbsp; Follow {{ article.author.username }}
      <span v-if="article.author.following" class="counter">({{ article.author.following }})</span>
    </button>
    &nbsp;&nbsp;
    <button class="btn btn-sm btn-outline-primary">
      <i class="ion-heart" />
      &nbsp; Favorite Article
      <span class="counter">({{ article.favoritesCount }})</span>
    </button>
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
