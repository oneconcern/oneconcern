<template lang="pug">
.featured
  .title {{ copys.featured }}
  .featured-posts
    .featured-post(v-for="(post, index) in posts_filtered", :key="index")
      a.featured-post-image(
        v-if="post.type === 'link'",
        :style="`background-image: url(${post.image})`",
        :href="post.link", target="_new")
      nuxt-link.featured-post-image(
        v-else,
        :style="`background-image: url(${post.image})`",
        :to="blog_path(post)")
      .featured-post-copy
        .featured-post-title {{ post.title }}
        .featured-post-author(v-if="post.author.name") {{ post.author.name }} - {{ post.author.position }}
        .featured-post-date {{ date(post.date) }}
</template>

<script>
import locale from '@/mixins/locale'
import inViewportDirective from 'vue-in-viewport-directive'
const getSlug = require('speakingurl')
export default {
  directives: { 'in-viewport': inViewportDirective },
  filters: {
    slug: function(title) {
      return getSlug(title)
    },
  },
  mixins: [ locale ],
  props: {
    posts: {
      required: true,
      type: Array,
    },
    copys: {
      type: Object,
      required: true,
    },
  },
  computed: {
    posts_filtered () {
      return this.posts.filter( entry => entry.locale && entry.locale.includes(this.locale) )
    },

  },
  methods: {
    slug (title) {
      return getSlug(title)
    },
  },
}
</script>

<style lang="stylus">

@import '../../../assets/stylus/guide/includes/*'

.featured
  padding 60px 0
  background-color white

  .title
    text-align center
    font-h4()
    padding 0 0 60px 0
    inViewportBottom()
.featured-posts
  max-width 1000px
  margin auto

  display flex
  flex-wrap wrap

.featured-post
  width 50%
  display flex
  justify-content flex-start
  flex-direction column
  height 400px

.featured-post-image
  height 220px
  background-size cover
  background-position 50% 50%

.featured-post-copy
  padding-top 30px
.featured-post-title
  color black
  font-h4()
  padding 0 30px 0 30px
.featured-post-author,
.featured-post-date
  font-s2()
  color mountain-mist
.featured-post-author
  padding 10px 0 0 30px
.featured-post-date
  padding 0 0 0 30px

@media all and (min-width: 1px) and (max-width: 1000px)
  .featured-post
    width 100%

</style>
