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
    <div class="card-content">
      <span class="card-title">
        <a :href="repo.owner.html_url" class="black-text">{{
          repo.owner.login
        }}</a>
        /
        <a :href="repo.html_url">{{ repo.name }}</a>
      </span>
      <p>{{ repo.description }}</p>
    </div>
    <div class="card-action flex-row">
      <a>
        <div class="badge green white-text stargazers">
          <i class="material-icons tiny">star_border</i>
          {{ repo.stargazers_count }}
        </div>
      </a>
      <a v-if="repo.homepage" :href="repo.homepage" class="green-text">
        Go to Homepage
      </a>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";

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
    return { user, repos };
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
