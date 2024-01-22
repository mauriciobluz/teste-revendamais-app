<template>
  <div class="address-form">
    <div class="form-group" v-if="search_toggle">
      <label class="form-label">Logradouro</label>
      <input type="text" v-model="street_name" class="form-control">
    </div>
    <div class="form-group" v-else>
      <label class="form-label">CEP</label>
      <input type="text" v-model="postal_code" v-maska data-maska="#####-###" class="form-control">
    </div>
    <div class="form-check form-switch mt-2">
      <input class="form-check-input" type="checkbox" role="switch" v-model="search_toggle" @change="resetInputs">
      <label class="form-check-label">Busca por Logradouro</label>
    </div>
    <div class="mt-3 d-flex justify-content-end">
      <button type="button" class="btn btn-primary" @click="searchAddressInfo" :disabled="!canSearch">Buscar</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import { vMaska } from "maska";

export default {
  directives: { maska: vMaska },
  data() {
    return {
      street_name: '',
      postal_code: '',
      search_toggle: false
    }
  },
  computed: {
    canSearch() {
      return this.search_toggle ? this.street_name != '' : this.postal_code.length == 9;
    }
  },
  methods: {
    resetInputs() {
      this.street_name = '';
      this.postal_code = '';
      this.$emit('resetResults');
    },
    searchAddressInfo() {      
      if (this.search_toggle) {
        axios.post('http://localhost:8000/api/postal-code/search-by-name', {search_text: this.street_name})
        .then( response => {
          this.$emit('changeResults', response.data);
        });
      } else {
        const sanitizedPostalCode = this.postal_code.replace(/[^0-9]/g, '');
        axios.get(`http://localhost:8000/api/postal-code/${sanitizedPostalCode}/search`)
        .then( response => {
          this.$emit('changeResults', response.data);
        })
        .catch( response => {
          this.$emit('errorSearch');
        });
      }
    }
  }
}
</script>
