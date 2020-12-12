<template>
  <div id="single-blog">
    <h1>{{ blog.title }}</h1>
    <article>{{ blog.content }}</article>
    <p>Author: {{ blog.author }}</p>
    <ul>
      <li :key="category.id" v-for="category in blog.categories">
        {{ category }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      id: this.$route.params.id,
      blog: {},
    };
  },
  created() {
    this.$http
      .get(
        `https://vue-blog-crypt0-default-rtdb.europe-west1.firebasedatabase.app/posts/${this.id}.json`
      )
      .then((data) => {
        return data.json();
      })
      .then((data) => {
        this.blog = data;
      });
  },
};
</script>

<style>
#single-blog {
  max-width: 960px;
  margin: 0 auto;
}
</style>