<template>
  <div class="AllPosts">
    <div id="post-list">
      <h1>All Posts</h1>
      <div class="container">
        <button v-if="authResult" @click="Logout" class="center">Logout</button>
      </div>
      <ul>
        <div class="item" v-for="post in posts" :key="post.id">
          <a class='singlepost' :href="'/apost/' + post.id">
            <span class="date"> <b>Date:</b> {{ post.date }} </span><br />
            <span class="body"> <b></b> {{ post.body }} </span> <br />
          </a>
        </div>
      </ul>
    </div>
  </div>
</template>


<script>
import auth from "../auth";

export default {
  name: "AllPosts",
  data() {
    return {
      posts: [],
      authResult: auth.authenticated(),
    };
  },
  methods: {
    fetchPosts() {
      fetch(`http://localhost:3000/api/posts/`)
        .then((response) => response.json())
        .then((data) => (this.posts = data))
        .catch((err) => console.log(err.message));
    },
    Logout() {
      fetch("http://localhost:3000/auth/logout", {
        credentials: 'include', //  Don't forget to specify this if you need cookies
      })
        .then((response) => response.json())
        .then((data) => {
          console.log(data);
          console.log('jwt removed');
          //console.log('jwt removed:' + auth.authenticated());
          this.$router.push("/login");
          //location.assign("/");
        })
        .catch((e) => {
          console.log(e);
          console.log("error logout");
        });
    },
  },
  mounted() {
    this.fetchPosts();
    console.log("mounted");
  },
};
</script>

<style scoped>
h1 {
  font-size: 20px;
}

a {
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

.item {
  background: rgb(189, 212, 199);
  margin-bottom: 5px;
  padding: 3px 5px;
  border-radius: 10px;
}

#post-list {
  background: #6e8b97;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.2);
  margin-bottom: 30px;
  padding: 10px 20px;
  margin: auto;
  width: 50%;
  border-radius: 20px;
}

#post-list ul {
  padding: 0;
}

#post-list li {
  display: inline-block;
  margin-right: 10px;
  margin-top: 10px;
  padding: 20px;
  background: rgba(255, 255, 255, 0.7);
}
</style>