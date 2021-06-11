<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">
        <div class="error" v-if="error" style="margin-bottom: 20px">
          <p>{{ error }}</p>
        </div>

        <search
          :value="search"
          placeholder="Type username..."
          @search="search = $event"
        />

        <button v-if="!repos" class="btn btnPrimary" @click="getRepos">
          Search!
        </button>
        <button v-else class="btn btnPrimary" @click="getRepos">
          Search again!
        </button>

        <div class="repos_wrapper" v-if="repos">
          <div class="repos_item" v-for="repo in repos" :key="repo.id">
            <div class="repos_info">
              <a class="link" target="_blank" :href="repo.html_url">{{
                repo.name
              }}</a>
              <span>{{ repo.stargazers_count }} ⭐</span>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import search from "@/components/Search.vue";
import axios from "axios";

export default {
  components: {
    search,
  },
  data() {
    return {
      search: "",
      error: null,
      repos: null,
    };
  },
  methods: {
    getRepos() {
      axios
        .get(`https://api.github.com/users/${this.search}/repos`)
        .then((res) => {
          this.repos = res.data;
          this.error = null;
        })
        .catch((err) => {
          this.repos = null;
          this.error = "Can`t find this user";
        });
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  display: flex;
  align-items: center;
  flex-direction: column;
}
button {
  margin-top: 20px;
}
.repos_wrapper {
  width: 400px;
  margin: 30px 0;
}

.repos_info {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 10px;
  padding: 10px 0;
  border-bottom: 1px solid #dbdbdb;
}
</style>