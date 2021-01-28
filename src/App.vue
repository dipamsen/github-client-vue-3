<template>
  <div class="container">
    <Repos />
  </div>
</template>

<script>
import Repos from "@/components/Repos";

export default {
  components: { Repos },
};
</script>

<style>
body {
  padding: 0;
  margin: 0;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}
#app {
  width: 100%;
  height: 100%;
  overflow: auto;
}
</style>
