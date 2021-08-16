<template>
  <div class='row lead my-1'>
    <div class='col-4'>{{Gasto.nombre}}</div>
    <div class='col-3 text-end'>{{Gasto.monto}}</div>
    <div class='col-3 text-end'>{{Gasto.tipo}}</div>
    <div class='col-1'>
      <div id='eliminar'  class='btn btn-primary '
            v-on:click='manejarClick($event,{indice:indice, id:id})'>Eliminar</div>
    </div>
    <div class='col-1'>
      <div id='editar'  class='btn btn-primary '
            v-on:click='manejarClick($event,{indice:indice, id:id})'>Editar</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Gasto',
  props: ['Gasto','id','indice'],
  data: function () {
    return {
      test: 'test'
    }
  },
  methods: {
    manejarClick: function (evento,GastoID) {
      if (evento.target.id==='eliminar'){
        this.$emit('eliminarGasto',GastoID)
      }
    }
  }
}
</script>

<style>

</style>
