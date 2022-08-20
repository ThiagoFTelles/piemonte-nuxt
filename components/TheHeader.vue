<template>
  <nav>
    <NuxtLink
      to="/"
      class="logo"
    >
      Home
    </NuxtLink>
    <NuxtLink
      class="btn"
      to="/fuzzy"
    >
      Fuzzy Search
    </NuxtLink>
  </nav>
</template>

<script>
export default {
  name: "TheHeader",
};
</script>

<style scoped>
nav {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 15px 20px;
  box-shadow: 0 2px 2px 4px rgba(30, 60, 90, 0.1);
}
</style>