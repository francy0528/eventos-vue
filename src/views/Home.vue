<template>
    <header>
        <h1>{{titulo}}</h1>
    </header>
    <main>
        <div>
            <h2>Registrar Evento</h2>
            <form id= "formulario_evento" action="#" method="post">
                <label for="cliente">Cliente:</label>
                <input type="text" name="cliente" v-model="evento.cliente">
                <label for="documento">Documento:</label>
                <input type="text" name="documento" v-model="evento.documento">
                <label for="salon">Salon:</label>
                <div>
                    <input type="radio" name="salon" value="0" v-model="salon"> Normal
                    <input type="radio" name="salon" value="1" v-model="salon"> Grande
                </div>
                <input type="checkbox" name="meseros_extra" v-model="extra">
                <label for="meseros_extra">Agregar meseros extra ?</label>
                <label v-if="extra" for="cantidad">Cantidad meseros</label>
                <input v-if="extra" type="number" name="cantidad" v-model="evento.meseros">
                <input type="checkbox" name="servicio_comida" v-model="comida">
                <label for="servicio_comida">Incluir comida ?</label>
                <label v-if="comida" for="platos">Cantidad platos</label>
                <input v-if="comida" type="number" name="platos" v-model="evento.platos">
                <button type="reset" name="limpiar">Limpiar</button>
                <button @click.prevent = "procesarInformacion" type="button">Agregar Eventos</button>
            </form>
            <h2>Meseros: {{extra}}</h2>
        </div>

        <div>
            <h2>Listado de Eventos</h2>

            <table border="1">
                <thead>
                    <tr>
                        <th>CLIENTE</th>
                        <th>SALON</th>
                        <th>MESEROS</th>
                        <th>PLATOS</th>
                        <th>TOTAL</th>
                    </tr>
                </thead>
                
                <TBody id="datos_eventos">
                    <tr v-for="unEvento in listaEventos" :key="unEvento">
                        <td>{{unEvento.cliente}}</td>
                        <td>{{unEvento.salon.nombre}}</td>
                        <td>{{unEvento.meseros}}</td>
                        <td>{{unEvento.platos}}</td>
                        <td>{{unEvento.total}}</td>                    
                    </tr>
                </TBody>
            </table>;
        </div>
    </main>
</template>

<script>
// @ is an alias to /src


export default {
  data(){
      return{
          titulo:"Gestion de Eventos",
          listaSalones:[
                    {"nombre":"Normal",
                    "precio":1000000,
                    "extra_mesero":150000,
                    "cantidad_meseros": 1,
                    "plato":50000
                    },
                    
                    {"nombre":"Grande",
                    "precio":2000000,
                    "extra_mesero":100000,
                    "cantidad_meseros": 4,
                    "plato":40000
                    }

          ],
          listaEventos:[
            {
                cliente :"jose",
                documento : "123456",
                meseros : 2,
                platos : 150,
                salon:  {"nombre":"Normal",
                        "precio":1000000,
                        "extra_mesero":150000,
                        "cantidad_meseros": 1,
                        "plato":50000},
                total: 3200000
                },
            {
                cliente :"laura",
                documento : "456789",
                meseros : 3,
                platos : 100,
                salon: {"nombre":"Grande",
                        "precio":2000000,
                        "extra_mesero":100000,
                        "cantidad_meseros": 4,
                        "plato":40000},
                total: 2200000
            }
        ],
        // diccionario
        evento:{
            cliente:"",
            documento:"",
            salon:{},
            meseros:0,
            platos:0,
            total:0
        },
        salon:-1,
        extra:false,
        comida:false,

      };
  },
  name: 'Home',
  components: {

  },
  methods:
  {
    procesarInformacion(){
        this.evento.salon = this.listaSalones[this.salon];
        if(this.extra){
            this.evento.meseros+=this.evento.salon.cantidad_meseros;
        }else{
            this.evento.meseros=this.evento.salon.cantidad_meseros;
        }
        if (!this.comida){
            this.platos=0;
        }
        let valorMeseros = this.evento.meseros * this.evento.salon.extra_mesero;
        let valorComida = this.evento.platos * this.evento.salon.plato;
        
        this.evento.total = valorComida + valorMeseros + this.evento.salon.precio;

        console.log(this.evento);
        
        this.listaEventos.push(this.evento);
    }  
  }
}
</script>
<style scoped>
body{
            padding: 2rem;
            }

        #formulario_evento{
            display: grid;
            grid-template-columns: 5rem 10rem;
            row-gap: 1rem;
            column-gap: 0.5rem;
        }
        main{
            display: grid;
            grid-template-columns: 1fr 3fr;
        }
        table{
            width: 80%;
            margin: 0 auto;
        }
        table td {
            text-align: center;
        }

</style>