<template>
  <div id="produto-table">
    <Message :msg="msg" v-show="msg" />
    <div>
      <div id="produto-table-heading">
        <div class="order-id">#:</div>
        <div>Produto:</div>
        <div>tipo:</div>
        <div>Data:</div>
        <div>Modelo:</div>
        <div>Ações:</div>
      </div>
    </div>
    <div id="produto-table-rows">
      <di class="produto-table-row" v-for="lista in listas" :key="lista.id" >
        <div class="order-number">{{lista.id}}</div>
        <div>{{ lista.produto }}</div>
        <div>{{ lista.tipo}}</div>
        <div>{{ lista.date_p}}</div>
        <div>
          <ul>
            <li v-for="(modelo, index) in lista.modelos" :key="index"> 
              {{modelo}}
            </li>
          </ul>
        </div>
        <div>
          <select name="status" id="status" @change="updateLista($event, lista.id)">
            <option value="">Selecione </option>
            <option v-for="s in status" :key="s.id" :value="s.tipo" :selected="lista.status == s.tipo">
              {{s.tipo}}
            </option>
          </select>
          <button class="delete-btn" @click="deleteLista(lista.id)">Cancelar</button>
        </div>
      </di>
   </div>
  </div>
</template>

<script>

import Message from './Message.vue';

export default {
  name: "Dashboard",
  data(){
    return {
      listas: null,
      lista_id: null,
      status: [],
      msg: null
    }
  }, 
  components: {
    Message
  },
  methods: {
    async getlista (){
      const req = await fetch("http://localhost:3000/listas");

      const data = await req.json();

      this.listas= data; 

      console.log(this.listas)

      //resgatar os status

      this.getStatus();

    },

    async getStatus(){

      const req = await fetch("http://localhost:3000/status");

      const data = await req.json();

      this.status = data;

      console.log(data);

    },

    async deleteLista(id){

      const req = await fetch(`http://localhost:3000/listas/${id}`, {
        method: "DELETE"
      });

      const res = await req.json();

      //msg

      // colocar uma msg de sistema
      this.msg = `Produto removido com sucesso`;

      // limpar mensagem de sistema
      setTimeout(()=> this.msg = "", 3000);

      this.getlista();

    },

    async updateLista(event, id){

      const option = event.target.value;

      const dataJson = JSON.stringify({status: option});

      const req = await fetch(`http://localhost:3000/listas/${id}`, {
        method: "PATCH",
        headers: {"Content-Type": "application/json"},
        body: dataJson
      });

      const res = await req.json();

      // colocar uma msg de sistema
      this.msg = `Produto Nº ${res.id} foi atualizado para ${res.status} !`;

      // limpar mensagem de sistema
      setTimeout(()=> this.msg = "", 3000);

      console.log(res)

    }
  },
  mounted() {

    this.getlista();


  }
};
</script>

<style scoped>
  #produto-table {
    max-width: 1200px;
    margin: 0 auto;
  }
  #produto-table-heading,
  #produto-table-rows,
  .produto-table-row {
    display: flex;
    flex-wrap: wrap;
  }
  #produto-table-heading {
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
  }
  .produto-table-row {
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid #CCC;
  }
  #produto-table-heading div,
  .produto-table-row div {
    width: 19%;
  }
  #produto-table-heading .order-id,
  .produto-table-row .order-number {
    width: 5%;
  }
  select {
    padding: 12px 6px;
    margin-right: 12px;
  }
  .delete-btn {
    background-color: #222;
    color:#fcba03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
  }
  
  .delete-btn:hover {
    background-color: transparent;
    color: #222;
  }
  
</style>