<template>
  <div class="main-container">
    <h1>Lista de Produtos: </h1>
    <ProdutoForm/>
  </div>
</template>

<script>
import ProdutoForm from "../components/ProdutoForm.vue"

export default {
  name: "HomeView",
  components: {
    ProdutoForm
  }
}

</script>
