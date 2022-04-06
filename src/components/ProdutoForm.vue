<template>
  <div>
    <Message :msg="msg" v-show="msg" />
    <div>
      <form id="produto-form" @submit="createProduto">
        <div class="input-container">
          <label for="nome">Produto: </label>
          <input
            type="text"
            name="nome"
            id="nome"
            v-model="produto"
            placeholder="Digite o produto"
            required
          />
        </div>
        <div class="input-container">
          <label for="tipo">Escolha o tipo: </label>
          <select name="tipo" id="tipo" v-model="tipo">
            <option value="">Selecione o tipo</option>
            <option v-for="tipo in tipos" :key="tipo.id" :value="tipo.tipo">
              {{tipo.tipo}}
            </option>
          </select>
        </div>
        <div id="modelos-container" class="input-container">
          <label id="modelos-title" for="modelos"
            >Selecione o modelo:
          </label>
          <div class="checkbox-container" v-for="modelo in modelosdata" :key="modelo.id">
            <input
              type="checkbox"
              name="modelos"
              v-model="modelos"
              :value="modelo.tipo"
            />
            <span>{{modelo.tipo}}</span>
          </div>
        </div>
        <div class="input-container">
          <input type="submit" class="submit-btn" value="Salvar" />
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import Message from './Message.vue';

export default {
  components: { Message },
  name: "ProdutoForm",
  data(){
    return{
      tipos: null,
      modelosdata: null,
      produto: null,
      date_p: null,
      tipo: null,
      modelos: [],
      msg: null

    }
  },
  methods: {
    async getIngredientes(){
      const req = await fetch("http://localhost:3000/Produtos")
      const data = await req.json();

      this.tipos = data.tipos;
      this.date_p = data.date_p;
      this.modelosdata = data.modelos;

    },

    async createProduto(e){

      e.preventDefault();

      const data = {
        produto : this.produto,
        date_p : new Date().toLocaleString(), 
        tipo: this.tipo,
        modelos: Array.from(this.modelos),
        status: "Em processo de compra"
      }

      const dataJson = JSON.stringify(data);

      const req = await fetch("http://localhost:3000/Listas", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: dataJson
      });

      const res = await req.json();

      // colocar uma msg de sistema
      this.msg = `Produto Nº ${res.id} realizado com sucesso`;

      // limpar mensagem de sistema
      setTimeout(()=> this.msg = "", 3000);


      // limpar os campos
      this.produto = "";
      this.date_p = "";
      this.tipo = "";
      this.modelos = [];

    
    }
  },
  mounted(){
    this.getIngredientes()
  },
  components: {
    Message
  }
};
</script>

<style scoped>

#produto-form {
  max-width: 400px;
  margin: 0 auto;
}

.input-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 40px;
}

label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #fcba03;
}

input,
select {
  padding: 5px 10px;
  width: 300px;
}

#modelos-container {
  flex-direction: row;
  flex-wrap: wrap;
}

#modelos-title {
  width: 100%;
}

.checkbox-container {
  display: flex;
  padding-top: 20px;
  align-items: flex-start;
  width: 50%;
}

.checkbox-container span,
.checkbox-container input {
  width: auto;
}

.checkbox-container span {
  margin-left: 6px;
  font-weight: bold;
}

.submit-btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}

.submit-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>