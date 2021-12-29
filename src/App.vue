<template>
  <div id="app">
    <div class="list">your list of items</div>
    <!-- add the component right after your list -->
    <div
      class="wrap"
      :class="{ 'can-scroll': scrollableContainer }"
      ref="scrollContainer"
    >
      <div class="users" v-if="users.length > 0">
        <div class="user" v-for="user in users" v-bind:key="{ user }">
          <img class="avatar" :src="user.url" height="40" alt="" />
          <h2>{{ user.id }}</h2>
          <h4>{{ user.title }}</h4>
        </div>
      </div>
      <mugen-scroll :handler="fetchData" :should-handle="!loading">
        loading...
      </mugen-scroll>
    </div>

    <!-- <div class="count">loaded {{ count }} time{{ count > 1 ? "s" : "" }}</div> -->
  </div>
</template>

<script>
import MugenScroll from "vue-mugen-scroll";

import http from "axios";

export default {
  name: "App",
  data() {
    // do not run handler when it's loading

    return {
      users: [],
      // count: 0,
      loading: false,
    };
  },
  methods: {
    async fetchData() {
      this.loading = true;
      // ... the code you wanna run to fetch data

      const result = await http
        .get("https://jsonplaceholder.typicode.com/posts", {
          params: {
            since:
              (this.users.length > 0 && this.users[this.users.length - 1].id) ||
              null,
          },
        })
        .then((res) => {
          return res.data;
        });
      this.users = [...this.users, ...result];
      console.log(this.users);
      // this.count++;
      this.loading = false;
    },
  },
  components: { MugenScroll },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
