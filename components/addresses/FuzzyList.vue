<template>
  <section class="addresses-container">
    <transition mode="out-in">
      <div
        v-if="searchResult && searchResult.length"
        class="addresses"
        key="addresses"
      >
        <div
          v-for="(address, index) in searchResult"
          :key="index"
          class="address"
        >
          <h2 class="title">
            {{address.logradouro}}
          </h2>
          <p class="details">
            {{address.localidade}} - {{address.uf}}
          </p>
        </div>
      </div>
      <div
        v-else-if="searchResult && searchResult.length === 0"
        key="without-results"
      >
        <p class="without-results">Tente fazer uma nova busca: Preencha os campos acima e clique na lupa.</p>
      </div>
      <LoadingPage
        v-else
        key="loading"
      />
    </transition>
  </section>
</template>

<script>
export default {
  name: "FuzzyList",
  data() {
    return {
      addresses: null,
      searchResult: [],
    };
  },
  methods: {
    async fuzzySearch() {
      try {
        this.searchResult = null;
        await this.$axios.$get(`/street-search${this.search}`).then((r) => {
          this.searchResult = r.data.length ? r.data : [];
        });
      } catch (e) {
        console.error(e);
      }
    },
  },
  computed: {
    search() {
      let searchInfo = "";
      for (let key in this.$route.query) {
        searchInfo += `/${this.$route.query[key]}`;
      }
      return this.$route.query ? searchInfo : null;
    },
  },
  watch: {
    search() {
      this.fuzzySearch();
    },
  },
};
</script>

<style scoped>
.addresses-container {
  max-width: 1000px;
  margin: 0 auto;
}

.addresses {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 30px;
  margin: 30px;
}

.address {
  background: #fff;
  border-radius: 4px;
  box-shadow: 0 4px 8px rgba(30, 60, 90, 0.1);
  padding: 10px;
  transition: all 0.2s;
  text-align: center;
}

.address:hover {
  box-shadow: 0 6px 12px rgba(30, 60, 90, 0.2);
  transform: scale(1.1);
  z-index: 1;
}

.addresses-container {
  max-width: 1000px;
  margin: 0 auto;
}

.without-results {
  text-align: center;
}

.title {
  margin-bottom: 10px;
  text-align: center;
}
</style>