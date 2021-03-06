<template>
  <Layout>
    <div class="post-title">
      <h1 class="post-title__text">
        {{ $page.post.title }}
      </h1>

      <PostMeta :post="$page.post" />

    </div>

    <div class="post content-box">
      <div class="post__header">
        <g-image alt="Cover image" v-if="$page.post.cover_image" :src="$page.post.cover_image" />
      </div>

      <div class="post__content" v-html="$page.post.content" />

      <div class="post__footer">
        <PostTags :post="$page.post" />
      </div>

      <div v-if="$page.post.comments" class="post-comments">
        <vue-disqus shortname="andrewrowland" :identifier="$page.post.title"></vue-disqus>
      </div>
    </div>

    <Author class="post-author" />
  </Layout>
</template>

<script>
import PostMeta from '~/components/PostMeta'
import PostTags from '~/components/PostTags'
import Author from '~/components/Author.vue'

export default {
  components: {
    Author,
    PostMeta,
    PostTags
  },
  metaInfo: function () {
    const { siteUrl } = this.$page.metadata

    const metaData = {
      title: this.$page.post.title,
      meta: [
        { name: 'description', content: this.$page.post.description },
        { name: 'twitter:card', content: 'summary_large_image' },
        { name: 'twitter:description', content: this.$page.post.description },
        { name: 'twitter:title', content: this.$page.post.title },
        { name: 'twitter:site', content: '@andyjrowland' },
        { name: 'twitter:creator', content: '@andyjrowland' },
        { property: 'og:type', content: 'article' },
        { property: 'og:title', content: this.$page.post.title },
        { property: 'og:description', content: this.$page.post.description },
        { property: 'og:url', content: `${siteUrl}${this.$page.post.path}` }
      ],
      script: [{ src: 'https://platform.twitter.com/widgets.js', async: true }]
    }

    if (this.$page.post.cover_image) {
      const { src: imagePath } = this.$page.post.cover_image
      const imageUrl = `${siteUrl}${imagePath}`

      metaData.meta.push({ name: 'twitter:image', content: imageUrl })
      metaData.meta.push({ property: 'og:image', content: imageUrl })
    }

    return metaData
  }
}
</script>

<page-query>
query Post ($id: ID!) {
  post: post (id: $id) {
    title
    path
    date (format: "D. MMMM YYYY")
    timeToRead
    tags {
      id
      title
      path
    }
    description
    content
    cover_image (width: 860, blur: 10)
    comments
  }
  metadata: metadata {
    siteUrl
  }
}
</page-query>

<style lang="scss">
.post-title {
  padding: calc(var(--space) / 2) 0 calc(var(--space) / 2);
  text-align: center;
}

.post {

  hr {
    opacity: 0.2;
    margin-top: calc(var(--space) / 1.5);
  }

  &__header {
    width: calc(100% + var(--space) * 2);
    margin-left: calc(var(--space) * -1);
    margin-top: calc(var(--space) * -1);
    margin-bottom: calc(var(--space) / 2);
    overflow: hidden;
    border-radius: var(--radius) var(--radius) 0 0;

    img {
      width: 100%;
    }

    &:empty {
      display: none;
    }
  }

  &__content {
    overflow: hidden;

    h2:first-child {
      margin-top: 0;
    }

    p:first-of-type {
      font-size: 1.2em;
      color: var(--title-color);
    }

    img {
      width: calc(100% + var(--space) * 2);
      margin-left: calc(var(--space) * -1);
      display: block;
      max-width: none;
    }
  }
}

.post-comments {
  margin-top: var(--space);

  &:empty {
    display: none;
  }
}

.post-author {
  margin-top: calc(var(--space) / 2);
}
</style>
