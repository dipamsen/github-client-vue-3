<template>
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
</template>

<script>
export default {
  props: ["repo"],
};
</script>

<style></style>
