<template>
  <div class="main-container">
    <h1>Lista de Produtos:</h1>
    <Dashboard />
  </div>
</template>

<script>
import Dashboard from '../components/Dashboard.vue'

export default {
  name: "ListaView", 
  components: { 
    Dashboard 
    },
}
</script>
