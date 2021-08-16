<template>
  <div v-if='logon' id='app' class='container'>
    <div class='text-dark text-center'>
      <h1>Mi libreta de gastos mensuales</h1>
    </div>
    <div class='row border border-dark rounded text-dack lead p-3 mt-4'>
      <div class='col-2 offset-3'>
        Nombre del Gasto
      </div>
      <div class='col-3'>
        <input v-model='nombreGasto' class='text-dack' type='text'>
      </div>
      <div class='col-2 offset-3'>
          Monto del Gasto
      </div>
      <div class='col-3'>
        <input v-model='montoGasto' class='text-dack' type='text'>
      </div>
      <div class='col-2 offset-3'>
          Tipo de Gasto
      </div>
      <div class='col-3'>
        <input v-model='tipoGasto' class='text-dack' type='text'>
      </div>
      <div class='col-2'>
        <div id='agregar' class='btn btn-primary' 
            v-on:click='manejarClick($event)'>Agregar Gastos</div>
      </div>
    </div>
    <div class='container border border-dark rounded text-dack p.3 mt-4'>
      <div class='row lead border border-dark rounded font-weight-bold'>
        <div class='col-4'>
          Nombre del Gasto
        </div>
        <div class='col-3 text-end'>
          Monto del Gasto
        </div>
        <div class='col-3 text-end'>
          Tipo de Gasto
        </div>
        <div class='col-2 text-center'>
          Accion
        </div>
      </div>
        
      <gastoComponente 
        v-for="(Gasto,index) in Gasto"
        v-bind:Gasto='Gasto'
        v-bind:id='Gasto.id'
        v-bind:indice='index'
        v-bind:key='index'
        v-on:eliminarGasto='eliminar($event)' ></gastoComponente>
    </div>
      <div class="container border border-dark rounded">
        <h4>Total :</h4>
      </div>
    </div>
  <div v-else>
    <loginForm v-bind:firebase="firebase"
                v-on:ingresoCorrecto='ingresoCorrecto($event)'></loginForm>
  </div>
</template>

<script>

import firebase from 'firebase'
import 'firebase/firestore'
import GastoComponente from './components/GastoComponente.vue'
import loginForm from './components/loginForm.vue'


export default {
  name: 'app',
  data: function() {
    return {Gasto : [],
    nombreGasto:'',
    montoGasto:'',
    tipoGasto:'',
    coleccion:{},
    logon: false,
    firebase:'',
    idUsuario:'',
    db:''
    }
  },
  methods: {
    manejarClick: function (evento) {
      if (evento.target.id==='agregar'){
        const GastoData = {nombre:this.nombreGasto,
                          monto:this.montoGasto,
                          tipo:this.tipoGasto}
        this.coleccion.add(GastoData)
        .then((docReference) => {
          this.Gasto.unshift({id:docReference.id, nombre: GastoData.nombre, monto: GastoData.monto, tipo: GastoData.tipo})
        })
        .catch((Errro) => {
          alert('No se pudo agregar el gasto al sistema. Error: '+Errro.message)
        })
        this.nombreGasto=''
        this.montoGasto=''
        this.tipoGasto=''
      }
    },
    eliminar: function (GastoID){
      this.coleccion.doc(GastoID.id).delete()
      this.Gasto.splice(GastoID.indice,1)
    },
    ingresoCorrecto: function(usuario) {
        console.log('User: '+usuario)
        this.idUsuario=usuario
        this.logon=true
        this.coleccion = this.db.collection('/usuarios/'+usuario+'/Gasto')
        this.coleccion.get()
        .then((Gasto)=>{
          Gasto.forEach((Gasto) => {
            this.Gasto.push({id:Gasto.id,nombre:Gasto.data().nombre,monto:Gasto.data().monto,tipo:Gasto.data().tipo})
        })
      })
    },
    
  },
  components: {
    GastoComponente, 
    loginForm
  },
  beforeMount: function () {
    var config = {
      apiKey: "AIzaSyAa9qJ-IGBqAukrHYB6YJjwa1TLdw6FIoo",
    authDomain: "vue-m-8f772.firebaseapp.com",
    projectId: "vue-m-8f772",
    storageBucket: "vue-m-8f772.appspot.com",
    messagingSenderId: "907258151068",
    appId: "1:907258151068:web:1d8d113c8453b23f716d59",
    measurementId: "G-QP7BYPZWPT"
    };
    firebase.initializeApp(config);
    this.db = firebase.firestore();
    const settings = {timestampsInSnapshots: true};
    this.db.settings(settings);
    this.firebase = firebase
  }
}
</script>


