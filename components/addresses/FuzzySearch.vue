<template>
  <form>
    <input
      name="street"
      class="search"
      v-model="street"
      placeholder="Logradouro"
    >
    <input
      name="city"
      class="search"
      v-model="city"
      placeholder="Cidade"
    >
    <input
      name="uf"
      class="search"
      :class="{error: error}"
      v-model="state"
      placeholder="UF"
    >
    <p
      class="error-message"
      v-if="error"
    >Preencha todos os campos acima</p>
    <input
      type="submit"
      id="submit"
      value="Buscar"
      @click.prevent="searchStreet"
    >
  </form>
</template>

<script>
export default {
  name: "FuzzySearch",
  data() {
    return {
      street: "",
      city: "",
      state: "",
      error: false,
    };
  },
  methods: {
    searchStreet() {
      if (!this.error) {
        this.$router.push({ query: { query: this.search } });
      }
    },
    validateData() {
      if (
        this.street.length < 3 ||
        this.city.length < 3 ||
        this.state.length != 2
      ) {
        return true;
      }
      return false;
    },
  },
  watch: {
    search() {
      this.error = this.validateData();
    },
  },
  computed: {
    search() {
      return `${this.state}/${this.city}/${this.street}`;
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
.search {
  width: 100%;
  padding: 20px;
  border: none;
}
.search.error {
  color: red;
}
.error-message {
  position: absolute;
}

.search:hover,
.search:focus {
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
  bottom: 0px;
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