<template>
  <Layout>
    <div
      id="main"
      class="alt">
      <section id="one">
        <div class="inner">
          <header class="major">
            <h1>{{ post.title }}</h1>
          </header>
          <div class="image main">
            <v-lazy-image
              :src="post.featuredImage.transformUrl"
              :src-placeholder="post.featuredImage.placeholder"
              :alt="post.title" />
          </div>
          <div v-html="post.text.content" />
        </div>
      </section>
    </div>
  </Layout>
</template>

<script>
export default {
  metaInfo () {
    const { title, description = `A post about ${title}` } = this.post.seo || this.post
    return {
      title,
      meta: [
        { hid: 'description', name: 'description', content: description }
      ]
    }
  },
  computed: {
    post () { return this.$page.post }
  }
}
</script>

<page-query>
query ($id: ID!) {
  post: datoCmsPost (id: $id) {
    title
    text {
      content
    }
    featuredImage {
      transformUrl(imgixParams: { h: 500, maxW: 1200, fit: "crop", q: 85, auto: "format,compress" })
      placeholder: transformUrl(imgixParams: { h: 300, w: 500, fit: "crop", q: 40, blur: 60, auto: "format,compress" })
    }
    seo {
      title
      description
    }
  }
}
</page-query>

<style lang="scss" scoped>
.image img {
  height: 500px;
}
</style>
