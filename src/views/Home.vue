<template>
  <div class="home">

    <v-row>
      <v-col xs='12' >

        <v-card :elevation=10 class="rounded-lg">
          <v-date-picker 
          v-model="fecha" 
          full-width 
          locale="es-uy"
          :min='minimo'
          :max='maximo'
          header-color="info"
          @change="getDolar(fecha)"
          ></v-date-picker>
        </v-card>
        <br>
        <v-card :loading="loading" :elevation=10 color='#2196F3' class="rounded-lg color-fondo" >
          <v-card-text  class="text-h3 text-center">{{valor}}</v-card-text>
        </v-card>
      </v-col>

    </v-row>

  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios';
export default {
  name: 'Home',
  data(){
    return{
      fecha: new Date().toISOString().substr(0,10),
      minimo:'1984',
      maximo: new Date().toISOString().substr(0,10),
      valor:null,
      loading : false
    }
  },
  methods:{
    async getDolar(dia){
      let arrayFecha = dia.split(['-']);
      let ddmmyy= arrayFecha[2]+'-'+arrayFecha[1]+'-'+arrayFecha[0];
      try{

        let datos = await axios.get(`https://mindicador.cl/api/dolar/${ddmmyy}`);

        /* console.log(datos.data.serie); */
        this.loading = true
        if(datos.data.serie.length > 0){

          setTimeout(() => {
            this.loading = false
            this.valor = datos.data.serie[0].valor;
          }, 1000);
          
          
          
        }else{
          setTimeout(() => {
            this.loading = false
            this.valor = 'Sin resultados'
          }, 2000);
          
        }

      }catch (error){
        console.log(error)
      }finally{
          
      }
    }
  },
  created(){
    this.getDolar(this.fecha);
  }
}
</script>

<style lang="css">
  .v-card__text{
    color:rgba(0, 0, 0, 1)!important;
  }
  .v-card{
    background-color: #ffff!important;
  }
</style>