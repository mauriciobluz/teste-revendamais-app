<template>
  <div class="results-list">
    <div class="info" v-if="this.results.length == 0">
      {{ message }}
    </div>
    <div class="result-item d-flex justify-content-between" v-for="item in this.results">
      <span>{{ item.street_name }}</span>
      <span>{{ item.postal_code }}</span>
    </div>
  </div>
</template>
  
<script>
export default {
  props: {
    results: {
      type: Array,
      required: false
    },
    message: {
      type: String,
      required: 'Sem Resultados'
    }
  },
  data() {
    return {
    }
  }
}
</script>

<style scoped>
.info {
  text-align: center;
  color: rgb(168, 167, 166);
}
.results-list {
  padding: 8px 8px;
  border-radius: 6px;
  background-color: rgb(248, 241, 241);
  max-height: 200px;
  overflow-y: auto;
}

.result-item {
  padding: 0px 8px;
  border-radius: 6px;
  margin-bottom: 5px;
  border-bottom: .5px solid rgb(224, 223, 223);
}

.result-item:hover {
  cursor: pointer;
  background-color: rgb(224, 223, 223);
}
</style>
