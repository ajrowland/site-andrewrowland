<template>
  <Layout>
    <div class="post-title">
      <h1 class="post-title__text">
        Page not found
      </h1>
    </div>

    <div class="post content-box">
        <p>Sorry, looks like that route does not exist. :-(</p>
    </div>
  </Layout>
</template>

<script>
export default {
  metaInfo: {
    title: '404'
  }
}
</script>

<style lang="scss">
.post-title {
  padding: calc(var(--space) / 2) 0 calc(var(--space) / 2);
  text-align: center;
}
</style>