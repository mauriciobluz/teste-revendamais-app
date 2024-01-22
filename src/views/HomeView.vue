<template>
  <main>
    <div class="card">
      <div class="card-body">
        <AddressForm @changeResults="showResults" @errorSearch="showError" @resetResults="resetSearch"/>
        <ResultsList class="mt-3" :results="this.results" :message="this.statusMessage"/>

      </div>
    </div>
  </main>
</template>

<script>
import AddressForm from '../components/AddressForm.vue'
import ResultsList from '../components/ResultsList.vue'

export default {
  components: {
    AddressForm,
    ResultsList
  },
  data() {
    return {
      results: [],
      statusMessage: 'Sem resultados'
    }
  },
  methods: {
    showResults(data) {
      this.results = Array.isArray(data) ? data : [data];
    },
    showError() {
      this.statusMessage = 'Algo deu errado na busca, revise os dados e tente novamente';
    },
    resetSearch() {
      this.statusMessage = 'Sem resultados';
      this.results = [];
    }
  }
}
</script>

<style scoped>
.card {
  width: 40vw;
  min-height: 30vh;
}
</style>