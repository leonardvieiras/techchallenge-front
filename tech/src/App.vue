<template>
  <div class="hero is-fullheight is-info is-bold">
  <div class="hero-body">
  <div class="container">

    <h1 class="title has-text-centered">Techechallenge - Intelipost e Penske</h1>
    <div class="box">

      <!-- our signup form ===================== -->
      <!--<form id="signup-form">

        <div class="field">
          <label class="label">Name</label>
          <input type="text" v-model="name" class="input" name="name">
        </div>


        <div class="field">
          <label class="label">Email</label>
          <input type="email" v-model="email" class="input" name="email">
        </div>


        <div class="field has-text-right">
          <button type="submit" class="button is-danger">Submit</button>
        </div>
      </form>-->
      <!--<ul id="example-1">
        <li v-for="item in itens.nfes">
          {{ item.status }}
        </li>
        <li v-for="item in itens.nfes">
          {{ item.status }}
        </li>
      </ul>-->
      <table style="width:100%">
  <tr>
    <th>Status</th>
    <th>Transporter id</th>
    <th>Nfe key</th>
    <th>Transaction id</th>
    <th>Transport unit id</th>
  </tr>
  <br>
  <tr v-for="item in itens.nfes">
    <td>{{item.status}}</td>
    <td class="tr_id">{{item.transporter_id}}</td>
    <td>{{item.nfe_key}}</td>
    <td class="tr_id">{{item.transaction_id}}</td>
    <td class="tr_id">{{item.transport_unit_id}}</td>
    <td>
      <section>
        <button :disabled="item.status === 'DISPATCHED' "class="button is-success is-medium"
            @click="fillModal(item)">
            Despachar
        </button>
        <b-modal :active.sync="isComponentModalActive" has-modal-card>
            <modal-form v-bind="formProps"></modal-form>
        </b-modal>
      </section>
    </td>
  </tr>
</table>
    </div>

  </div>
  </div>
  </div>

</template>

<script>
import Vue from 'vue';
import Buefy from 'buefy';
import 'buefy/lib/buefy.css';
import axios from 'axios';
Vue.use(Buefy);

const ModalForm = {
      props: ['status', 'transporter_name','transporter_id', 'nfe_key', 'transaction_id', 'transport_unit_id'],
      template: `
          <form action="">
              <div class="modal-card" style="width: auto">
                  <header class="modal-card-head">
                      <p class="modal-card-title">Informações:</p>
                  </header>
                  <section class="modal-card-body">
                      <b-field label="Status">
                          <b-input
                              :disabled=true
                              type="email"
                              :value="status"
                                placeholder="Status"
                              required>
                          </b-input>
                      </b-field>

                      <b-field  label="Transporter name">
                          <b-input
                              :disabled=true
                              type="transporter_name"
                              :value="transporter_name"
                              password-reveal
                              placeholder="Transporter name"
                              required>
                          </b-input>
                      </b-field>

                      <b-field  label="Transporter id">
                          <b-input
                              :disabled=true
                              type="transporter_id"
                              :value="transporter_id"
                              password-reveal
                              placeholder="Transporter id"
                              required>
                          </b-input>
                      </b-field>

                      <b-field  label="Nfe key">
                          <b-input
                              :disabled=true
                              type="nfe_key"
                              :value="nfe_key"
                              password-reveal
                              placeholder="Nfe key"
                              required>
                          </b-input>
                      </b-field>


                                            <b-field  label="Transaction id">
                                                <b-input
                                                    :disabled=true
                                                    type="transaction_id"
                                                    :value="transaction_id"
                                                    password-reveal
                                                    placeholder="Transaction id"
                                                    required>
                                                </b-input>
                                            </b-field>

                                            <b-field  label="Transport unit id">
                                                <b-input
                                                    :disabled=true
                                                    type="transport_unit_id"
                                                    :value="transport_unit_id"
                                                    password-reveal
                                                    placeholder="Transport unit id"
                                                    required>
                                                </b-input>
                                            </b-field>



                  </section>
              </div>
          </form>

      `


  }

export default {
  name: 'App',
  components: {
    ModalForm,
  },
  data(){0
    return{
      isComponentModalActive: false,
      formProps: {
        status: 'evan@you.com',
        transporter_name: 'testing',
        transporter_id: 'transporter id',
        nfe_key:'TESTE',
        transaction_id:'Teste 2',
        transport_unit_id:'Transport unit id'
      },
      itens: {},
    }
  },
  methods: {
    teste(){
      console.log("TESTe");
    },
    getNfes(){
      axios.get('http://10.100.20.224:8080/nfes').then((response) =>{
        this.itens = response.data;
      });

    },
    fillModal(item){
      item.status = "DISPATCHED";
      this.isComponentModalActive = true;
      this.formProps.status = item.status;
      this.formProps.transporter_id = item.transporter_id;
      this.formProps.nfe_key = item.nfe_key;
      this.formProps.transaction_id = item.transaction_id;
      this.formProps.transport_unit_id =item.transport_unit_id;

      axios.post("http://10.100.20.224:8080/dispatch",
      {
        "transport_unit_id": item.transport_unit_id,
        "transaction_id": item.transaction_id,
        "nfe_key": item.nfe_key,
        "transporter_id": item.transaction_id,
        "transporter_name": item.transporter_name,
        "status": item.status,
      }
    ).then((response) =>{
      console.log(response.data);
    });
    }
  },
  beforeMount() {
    axios.get('http://10.100.20.224:8080/nfes').then((response) =>{
      this.itens = response.data;
    });
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

button{
  margin-bottom: 10px;
  margin-left: 10px;
  font-size: 10px !important;
}
tr{
  font-size: 12px;
}
.tr_id{
  font-size:8px;

}
th{
  font-size: 15px;
}
</style>
