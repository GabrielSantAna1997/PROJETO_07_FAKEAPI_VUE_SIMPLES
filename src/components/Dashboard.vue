<template>
  <div id="burger-table" v-if="burgers">
    <p>{{ metodo() }}</p>
    <Message :msg="msg" v-show="msg" />
    <div>
      <div id="burger-table-heading">
        <div class="order-id">#:</div>
        <div>Cliente:</div>
        <div>Pão:</div>
        <div>Carne:</div>
        <div>Opcionais:</div>
        <div>Ações:</div>
      </div>
    </div>
    <div id="burger-table-rows">
      <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
        <div class="order-number">{{ burger.id }}</div>
        <div><a class="DinDin" step=".01">R${{ burger.valor }}</a> 💵 {{ burger.nome }}</div>
        <div>{{ burger.pao }}</div>
        <div>{{ burger.carne }}</div>
        <div>
          <ul v-for="(opcional, index) in burger.opcionais" :key="index">
            <li>{{ opcional }}</li>
          </ul>
        </div>
        <div>
          <select
            name="status"
            class="status"
            @change="updateBurger($event, burger.id)"
          >
            <option
              :value="s.tipo"
              v-for="s in status"
              :key="s.id"
              :selected="burger.status == s.tipo"
            >
              {{ s.tipo }}
            </option>
          </select>
          <button class="delete-btn" @click="deleteBurger(burger.id)">
            Cancelar
          </button>
        </div>
      </div>
    </div>
  </div>
  <div v-if="burgers == 0" class="naotem">
    <h2>Não há pedidos no momento!</h2>
  </div>
</template>

<script>
import Message from "./Message.vue";

export default {
  name: "Dashboard",
  data() {
    return {
      burgers: null,
      burger_id: null,
      status: [],
      msg: null,
    };
  },
  components: {
    Message,
  },
  methods: {
    metodo(value) {
      let i = 0;
      let carnes = null;
      let bread = null;
      let opcionais = null;
      let valorCarne = [];
      let valortotal = [];
      let valorpao = [];

      for (i = 0; this.burgers[i]; i++) {
        if (this.burgers[i]) {
          carnes = this.burgers[i].carne;
          if (carnes == "Alcatra" || carnes == "Picanha") {
            valorCarne[i] = 10.50;
          } else if (
            carnes == "Contrafilé" ||
            carnes == "Maminha" ||
            carnes == "Patinho"
          ) {
            valorCarne[i] = 8.50;
          } else if (carnes == "Veggie burger") {
            valorCarne[i] = 12.50;
          }
        }
        if (this.burgers) {
          bread = this.burgers[i].pao;
          if (bread == "Italiano Branco" || bread == "Integral") {
            valorpao[i] = valorCarne[i] + 5.50;
          } else if (bread == "3 Queijos" || bread == "Parmesão e Orégano") {
            valorpao[i] = valorCarne[i] + 6.20;
          }
        }
        if (this.burgers[i].opcionais) {
          opcionais = this.burgers[i].opcionais.length;
          opcionais = opcionais * 2.00;
          valortotal[i] = valorpao[i] + opcionais;
        }

        this.burgers[i].valor = valortotal[i]

      }
    },

    //------------------VERIFICAR AS INFORMAÇÕES PARA UTILIZAR NO HMTL---------------//
    async getPedidos() {
      const req = await fetch("http://localhost:8083/burgers");
      const data = await req.json();
      this.burgers = data;
      // Resgata os status de pedidos
      this.getStatus();
    },

    //------------------VERIFICAR AS INFORMAÇÕES DE STATUS---------------//
    async getStatus() {
      const req = await fetch("http://localhost:8083/status");
      const data = await req.json();
      this.status = data;
    },

    //------------------DELETAR---------------//
    async deleteBurger(id) {
      const req = await fetch(`http://localhost:8083/burgers/${id}`, {
        method: "DELETE",
      });
      const res = await req.json();
      this.getPedidos();

      //COLOCAR UMA MENSAGEM DE SISTEMA
      this.msg = `Pedido Deletado!`;

      //Limpar msg
      setTimeout(() => (this.msg = ""), 5000);
    },

    //------------------ATUALIZAR---------------//
    async updateBurger(event, id) {
      const option = event.target.value;
      const dataJson = JSON.stringify({ status: option });
      const req = await fetch(`http://localhost:8083/burgers/${id}`, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });
      const res = await req.json();
      //console.log(res);
      //COLOCAR UMA MENSAGEM DE SISTEMA
      this.msg = `Pedido Nº ${res.id} foi atualizado para ${res.status}!`;

      //Limpar msg
      setTimeout(() => (this.msg = ""), 5000);
    },
  },
  mounted() {
    this.getPedidos();
  },
};
</script>

<style scoped>
#burger-table {
  max-width: 1200px;
  margin: 0 auto;
}
#burger-table-heading,
#burger-table-rows,
.burger-table-row {
  display: flex;
  flex-wrap: wrap;
}
#burger-table-heading {
  font-weight: bold;
  padding: 12px;
  border: 3px solid #333;
  background-color: #222;
  color: #ffffff;
}
.burger-table-row {
  margin-top: 10px;
  width: 100%;
  padding: 5px;
  border: 2px solid #ccc;
  align-items: center;
}
#burger-table-heading div,
.burger-table-row div {
  width: 19%;
}
#burger-table-heading .order-id,
.burger-table-row .order-number {
  width: 2%;
}
select {
  padding: 12px 6px;
  margin-right: 12px;
}
.delete-btn {
  background-color: #222;
  color: #ffffff;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  transition: 0.5s;
}

.delete-btn:hover {
  background-color: transparent;
  color: #222;
}

.naotem {
  margin: 0;
  padding: 20px;
  text-align: center;
  background-color: aliceblue;
}

.DinDin {
  padding: 5px;
  border: 1px solid rgb(8, 97, 35);;
  color: rgb(8, 97, 35);
}
#passar_mouse:hover #mostrar{
  display:block;
  }
</style>