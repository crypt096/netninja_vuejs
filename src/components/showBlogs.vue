<template>
  <div id="show-blogs">
    <h1>All Blog Articles</h1>
    <input type="text" v-model="search" placeholder="Search for blog..." />
    <div :key="blog.id" v-for="blog in filterBlogs" class="single-blog">
      <router-link v-bind:to="'/blog/' + blog.id">
        <h2>{{ blog.title | uppercase }}</h2>
      </router-link>
      <article>{{ blog.content | snippet }}</article>
    </div>
  </div>
</template>

<script>
import searchMixin from "../mixins/searchMixin";
export default {
  data() {
    return {
      blogs: [],
      search: "",
    };
  },
  methods: {},
  created() {
    this.$http
      .get(
        "https://vue-blog-crypt0-default-rtdb.europe-west1.firebasedatabase.app/posts.json"
      )
      .then((data) => {
        return data.json();
      })
      .then((data) => {
        let blogsArray = [];
        for (let key in data) {
          data[key].id = key;
          blogsArray.push(data[key]);
        }
        this.blogs = blogsArray;
      });
  },
  mixins: [searchMixin],
  filters: {
    uppercase(value) {
      return value.toUpperCase();
    },
    snippet(value) {
      return value.slice(0, 100).concat("...");
    },
  },
  directives: {
    rainbow: {
      bind(el, binding, vnode) {
        el.style.color = "#" + Math.random().toString().slice(2, 8);
      },
    },
    theme: {
      bind(el, binding, vnode) {
        if (binding.value === "wide") {
          el.style.maxWidth = "1200px";
        } else if (binding.value === "narrow") {
          el.style.maxWidth = "560px";
        }
        if (binding.arg === "column") {
          el.style.background = "#ddd";
          el.style.padding = "20px";
        }
      },
    },
  },
};
</script>

<style>
#show-blogs {
  max-width: 800px;
  margin: 0 auto;
}

.single-blog {
  padding: 20px;
  margin: 20px 0;
  box-sizing: border-box;
  background: #eee;
}
</style>