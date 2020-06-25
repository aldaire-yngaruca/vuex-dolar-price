<template>
  <v-layout :wrap="true">
    <v-flex xs12>
      <v-card>
        <v-date-picker v-model="picker" full-width locale="es-cl"
        :min="minimo" :max="maximo" @change="getDolar(picker)">
        </v-date-picker>
      </v-card>
      <v-card color="error" dark>
        <v-card-text class="display-1 text-xs-center">
          {{valor}}
        </v-card-text>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
// @ is an alias to /src
import axios  from 'axios';
export default {
  name: 'Home',
  components: {
  },
  data(){
    return {
      picker:new Date().toISOString().substr(0,10),
      minimo:'1984',
      maximo:new Date().toISOString().substr(0,10),
      valor :null
    }
  },
  methods:{
    async getDolar(dia){
      let arrayFecha = dia.split(['-'])
      let ddmmyy = arrayFecha[2]+'-'+arrayFecha[1]+'-'+arrayFecha[0]
      try{

        let datos = await this.axios.get(`https://mindicador.cl/api/dolar/${ddmmyy}`)
        if(datos.data.serie.length > 0 ){
          this.valor = datos.data.serie[0].valor
        }
        else{
          this.valor = 'Sin resultados'
        }

      }catch(e){
        console.log(e)
      }
      finally{

      }
      
    }
  },
  created(){
    this.getDolar('01-02-2019')
  }
}
</script>
