<template>
  <section class="src-componentes-conversor">
    <div class="jumbotron">
      <h1>Conversor a dólares</h1>
      <br />
      <h4>Ingrese monto $ <input type="number" v-model="numero" /></h4>
      <h4>
        Valor del dólar en $ <input type="number" v-model="cotizacion" /> -
        Actualización
        <input
          type="Checkbox"
          @click="actualizarABlue()"
          v-model="cotizacion"
        />
        <span v-if="blue" class="ml-3">{{ new Date().toLocaleString() }}</span>
      </h4>
      <h4>Valor convertido USD {{ numero | convertir(cotizacion) }}</h4>
      <br>
      <br>
      <br>
      <p>Pregunta 1/5: c)</p>
      <p>Pregunta 2/5: b)</p>
      <p>Pregunta 3/5: c)</p>
      <p>Pregunta 4/5: a)</p>
      <p>Pregunta 5/5: b)</p>
    </div>
  </section>
</template>

<script lang="js">

  export default  {
    name: 'src-componentes-conversor',
    props: [],
    mounted () {
      this.getInitialData()
    },
    data () {
      return {
        url: 'https://www.dolarsi.com/api/api.php?type=valoresprincipales',
        numero: "",
        cotizacion: "",
        blue: false,
        timer: null,
      }
    },
   
    methods: {
      getInitialData(){
        try{
        this.numero= "",
        this.cotizacion= "",
        this.blue= false,
        this.timer= null
        }
        catch(error){
          console.error('Ocurrió un error al inicializar los datos')
        }
      },
        async pedirAlServidor(){
        try{
        let respuesta = await this.axios(this.url)
        let datos = respuesta.data
        let cot = datos.find(dato => dato.casa.nombre == 'Dolar Blue')
        this.cotizacion = Number(cot.casa.venta.replace(',','.'))
      }
      catch(error){
        console.error('Ocurrió un error al realizar la solicitud al servidor')
      }
      },

       actualizarABlue() {
        this.blue = !this.blue
        console.log(this.blue)
        if(this.blue){
          this.timer = window.setInterval(this.pedirAlServidor(), 2000)
        }else{
          clearInterval(this.timer)
        }

    },
      },
       filters:{
      convertir(numero, cotizacion){
        if(cotizacion != 0){
          return (numero/cotizacion).toFixed(2)
        }
      },
    },
      
      
    computed: {
      
    },
    }


</script>

<style scoped lang="css">
.src-componentes-conversor {
}
h1{
  text-align: center;
}
.jumbotron{
  text-align: left;
}
</style>
