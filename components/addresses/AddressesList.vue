<template>
  <section class="addresses-container">
    <transition mode="out-in">
      <div v-if="searchResult">
        <div
          :key="searchResult.postal_code"
          class="address"
        >
          <h2 class="title">
            {{searchResult.street}}
          </h2>
          <p class="details">
            {{searchResult.city}} - {{searchResult.state}}
          </p>
          <p class="details">
            CEP {{searchResult.postal_code}}
          </p>
        </div>
      </div>
      <div
        v-if="addresses && addresses.length"
        class="addresses"
        key="addresses"
      >
        <div
          v-for="address in addresses"
          :key="address.id"
          class="address"
        >
          <router-link :to="{name: 'address', params:{id: address.id, street:address.street} }">
            <h2 class="title">
              {{address.street}}
            </h2>
            <p class="details">
              {{address.city}} - {{address.state}}
            </p>
          </router-link>
        </div>
      </div>
      <div
        v-else-if="addresses && addresses.length === 0"
        key="without-results"
      >
        <p class="without-results">Busca sem resultados. Tente buscar outro CEP.</p>
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
  name: "AddressesList",
  data() {
    return {
      addresses: null,
      searchResult: null,
    };
  },
  methods: {
    async getAddresses() {
      await this.$axios.$get(`/addresses`).then((r) => {
        this.addresses = r.addresses.original;
      });
    },
    searchAddresses() {
      this.addresses = null;
      this.$axios.$get(`/postal-code-search/${this.postalCode}`).then((r) => {
        this.searchResult = r.address.original;
      });
    },
  },
  computed: {
    postalCode() {
      let queryString = "";
      for (let key in this.$route.query) {
        queryString += `${this.$route.query[key]}`;
      }
      return queryString ? queryString : null;
    },
  },
  watch: {
    postalCode() {
      this.searchAddresses();
    },
  },
  created() {
    this.getAddresses();
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