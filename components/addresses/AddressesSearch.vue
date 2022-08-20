<template>
  <form>
    <input
      name="search"
      id="search"
      :class="{error: error}"
      type="number"
      v-model="search"
      placeholder="Buscar CEP"
    >
    <p v-if="error">Digite o CEP com 8 números</p>
    <input
      type="submit"
      id="submit"
      value="Buscar"
      @click.prevent="searchAddresses"
    >
  </form>
</template>

<script>
export default {
  name: "AddressesSearch",
  data() {
    return {
      search: "",
      error: false,
    };
  },
  methods: {
    searchAddresses() {
      if (!this.error && this.search.length) {
        this.$router.push({ query: { query: this.search } });
      }
    },
  },
  watch: {
    search() {
      let length = this.search.length;
      this.error = length > 0 && length != 8 ? true : false;
    },
  },
};
</script>

<style scoped>
form {
  max-width: 600px;
  margin: 30px auto 60px auto;
  position: relative;
  padding: 0 10px;
}
#search {
  width: 100%;
  padding: 20px;
  border: none;
}
#search.error {
  color: red;
}
#search:hover,
#search:focus {
  transform: scale(1.1);
}
#submit {
  background: url("../../static/icons/search.svg") no-repeat center center;
  border: none;
  box-shadow: none;
  cursor: pointer;
  height: 62px;
  position: absolute;
  width: 60px;
  right: 0px;
  text-indent: -150px;
  top: 0px;
}

/* Hide input arrows */
/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type="number"] {
  -moz-appearance: textfield;
}
</style>