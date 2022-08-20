<template>
  <section>
    <h1>Pesquisa por Logradouro</h1>
    <AddressesFuzzySearch />
    <AddressesFuzzyList />
  </section>
</template>


<script>
export default {
  name: "FuzzySearchPage",
};
</script>

<style scoped>
h1 {
  font-size: 2rem;
  color: #91549e;
  margin-top: 40px;
  text-align: center;
}
</style>