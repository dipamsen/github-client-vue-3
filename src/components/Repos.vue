<template>
  <div class="card info">
    <a :href="user.html_url" class="black-text">
      <div class="card-content">
        <div class="flex-row">
          <div class="icon">
            <img :src="user.avatar_url" alt="" class="circle" />
          </div>
          <div class="card-title">
            <strong>
              <span class="big">{{ user.name }}</span>
            </strong>
          </div>
        </div>
        {{ user.bio }}
      </div>
    </a>
  </div>
  <div v-for="repo in repos" :key="repo.id" class="card hoverable">
    <Repo :repo="repo" />
  </div>
</template>

<script>
import { ref } from "vue";
import Repo from "@/components/Repo.vue";

export default {
  setup() {
    const user = ref({});
    const repos = ref([]);
    let userName =
      new URLSearchParams(location.search).get("username") || "processing";
    async function getRepos() {
      const userUrl = `https://api.github.com/users/${userName}`;

      user.value = await fetch(userUrl).then((data) => data.json());
      const reposUrl = user.value.repos_url;
      const repoList = await fetch(reposUrl).then((data) => data.json());
      repos.value = repoList.sort((a, b) => {
        if (a.pinned) return -1;
        if (b.pinned) return 1;
        return b.stargazers_count - a.stargazers_count;
      });
    }
    getRepos();
    return { user, repos, Repo };
  },
};
</script>

<style>
.big {
  font-size: larger;
}
.icon img {
  height: 75px;
  margin: 10px;
}
.flex-row {
  display: flex;
  align-items: center;
  align-content: center;
}
.stargazers {
  display: flex;
  align-items: center;
  padding: 6px;
  border-radius: 10px;
}
</style>
