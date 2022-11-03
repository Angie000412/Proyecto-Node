
<template>
    <v-data-table
      
      :headers="headers"
      :items="rsus"
      :search="search"
      sort-by="calories"
      class="elevation-18 mx-auto"
      
      
    >
      <template #top>
        <v-toolbar flat>
          <v-toolbar-title><v-icon>mdi-map-marker-radius</v-icon>Geolocalizar</v-toolbar-title>
             <v-divider class="mx-4" inset vertical></v-divider>
          <v-text-field
          v-model="search"
          class="justify-center"
          append-icon="mdi-magnify"
          label="Search"
          single-line
          hide-details
        ></v-text-field>
          <v-divider class="mx-4" inset vertical></v-divider>
         <!-- <v-btn
         color="primary"
         text
         elevation="18"
      
         @click="VerTodas"
         >
        Ver Todo
         </v-btn> -->
        </v-toolbar>
        
      </template>
      <!-- eslint-disable-next-line vue/valid-v-slot -->
    <template #item.actions="{ item }">
      <v-btn color="green" x-small @click="Geolocalizar(item)"><v-icon small  color="white"> mdi-map-marker-radius </v-icon></v-btn>
    </template>
    </v-data-table>
  </template>
  
  <script>
import { latLng } from 'leaflet';

  
  
  
  
  const axios = require('axios')
  import router from 'vue-router'
  export default {
    beforeRouteEnter:(to,from,next)=>{
      console.log("[IN-component] beforeEnter guard");
      let isAuth = sessionStorage.getItem("username");
      let role = sessionStorage.getItem("rol");    
      return isAuth && role==='Especialista' ? next() : next("/login")  
    },
    
    data: () => ({
      search: '',
      
      dialog: false,
      dialogDelete: false,
      autoCompleteInput: {},
      Autocompletar: {},
      geoloc:[],
      
      headers: [
        /* {
          text: 'id',
          align: 'start',
          value: 'id',
        }, */
        { text: 'Tipo de la Entidad', value: 'tipo_entidad' , sortable: false},
        { text: 'Provincia', value: 'provincia', sortable: false },
        { text: 'Municipio', value: 'municipio' , sortable: false},
        { text: 'Tipo de Residuo', value: 'tipo_residuo' , sortable: false},
        { text: 'Nombre de la Entidad', value: 'nombre_entidad', sortable: false },
        
       
  
        
       
        { text: 'Latitud', value: 'latitud', sortable: false },
        { text: 'Longitud', value: 'longitud' , sortable: false},
        { text: 'Geolocalizar', value: 'actions', align:'center', sortable: false },
      ],
      rsus: [],
      auto:[],
      datos:{},
      editedIndex: -1,
      editedItem: {
        id: 0,
                nombre_entidad: '',
                pdprom: 0,
                alimentos: 0,
                calor: 0,
                elec_min: 0,
                elec_max: 0,

               
               
                provincia: '',
                municipio: '',
                latitud: '',
                longitud: '',
                tipo_residuo:'',
                
              

              
              
    },
     
      entidades:[
        'cuniculas',
        'porcinos',
'ganadomayors',
'avicolas',
'ganadomenors',
'bebidaslicores',
'arroceras',
'azucareras',
'granos',
'pescas',
'carnicas',
'rsus',
'conservas',
'lacteas',
        
      ],
     
            
     
    }),
  
  
      
   /*  watch: {
      dialog(val) {
        val || this.close()
      },
      dialogDelete(val) {
        val || this.closeDelete()
      },
    }, */
      
    created() {
      this.initialize()
    },
  
    methods: {
        Geolocalizar(item){
            //aqui lo que quiero hacer es recoger en un arreglo 
            //la latlng del item que toqué el botón,
            // y también los datos del potencial
            this.editedIndex = this.rsus.indexOf(item)
            this.editedItem = Object.assign({}, item)
            var lat = this.editedItem.latitud
            var lng = this.editedItem.longitud
             this.datos = {
                "potencialPromedio": this.editedItem.pdprom,
                "alimentos":  this.editedItem.alimentos,
                "calor": this.editedItem.calor,
                "elecMin": this.editedItem.elec_min,
                "elecMax": this.editedItem.elec_max,
                "nombreEntidad": this.editedItem.nombre_entidad
            }
            console.log(`${lat}, ${lng}`);

            console.log(this.datos.potencialPromedio, this.datos.alimentos, 
            this.datos.calor, this.datos.elecMin, this.datos.elecMax, this.datos.nombreEntidad);
           this.$router.push('/elMapa') 
  
        },
/* /*  VerTodas(){

//aquí lo que quiero es crear un arreglo con todas las latlng de todas las entidades
this.$router.push('/OtroMapaConTodo')

 
 },
 */


      async initialize() {
        // eslint-disable-next-line no-unused-vars
    

        await axios
        
          .get('http://localhost:1337/api/cuniculas/')
      
          .then((response) => {
            response.data.data.forEach((item) => {
              this.rsus.push({
  
                id: item.id,
                nombre_entidad: item.attributes.nombre_entidad,
                pdprom: item.attributes.pdprom,
                alimentos: item.attributes.alimentos,
                calor: item.attributes.calor,
                elec_min: item.attributes.elec_min,
                elec_max: item.attributes.elec_max,

               
               
                provincia: item.attributes.provincia,
                municipio: item.attributes.municipio,
                latitud: item.attributes.latitud,
                longitud: item.attributes.longitud,
                tipo_residuo:item.attributes.tipo_residuo,
                
                tipo_entidad: 'Cunícula'
              })            
            })
          })
          .catch((error) => {
            console.error(error)
          });

        await axios
        
          .get('http://localhost:1337/api/avicolas/')
      
          .then((response) => {
            response.data.data.forEach((item) => {
              this.rsus.push({
  
                id: item.id,
                nombre_entidad: item.attributes.nombre_entidad,
                pdprom: item.attributes.pdprom,
                alimentos: item.attributes.alimentos,
                calor: item.attributes.calor,
                elec_min: item.attributes.elec_min,
                elec_max: item.attributes.elec_max,
               
                provincia: item.attributes.provincia,
                municipio: item.attributes.municipio,
                latitud: item.attributes.latitud,
                longitud: item.attributes.longitud,
                tipo_residuo:item.attributes.tipo_residuo,
                
                tipo_entidad: 'Avícolas'
              })            
            })
          })
          .catch((error) => {
            console.error(error)
          });
        await axios
        
          .get('http://localhost:1337/api/porcinos/')
      
          .then((response) => {
            response.data.data.forEach((item) => {
              this.rsus.push({
  
                
                nombre_entidad: item.attributes.nombre_entidad,
                pdprom: item.attributes.pdprom,
                alimentos: item.attributes.alimentos,
                calor: item.attributes.calor,
                elec_min: item.attributes.elec_min,
                elec_max: item.attributes.elec_max,
               
                provincia: item.attributes.provincia,
                municipio: item.attributes.municipio,
                latitud: item.attributes.latitud,
                longitud: item.attributes.longitud,
                tipo_residuo:item.attributes.tipo_residuo,
                
                tipo_entidad: 'Porcinas'
              })            
            })
          })
          .catch((error) => {
            console.error(error)
          });
          await axios
        
        .get('http://localhost:1337/api/ganadomayors/')
    
        .then((response) => {
          response.data.data.forEach((item) => {
            this.rsus.push({

             
              nombre_entidad: item.attributes.nombre_entidad,
              pdprom: item.attributes.pdprom,
                alimentos: item.attributes.alimentos,
                calor: item.attributes.calor,
                elec_min: item.attributes.elec_min,
                elec_max: item.attributes.elec_max,
             
              provincia: item.attributes.provincia,
              municipio: item.attributes.municipio,
              latitud: item.attributes.latitud,
              longitud: item.attributes.longitud,
              tipo_residuo:item.attributes.tipo_residuo,
              
              tipo_entidad: 'Ganado Mayor'
            })            
          })
        })
        .catch((error) => {
          console.error(error)
        });
        await axios
        
        .get('http://localhost:1337/api/ganadomenors/')
    
        .then((response) => {
          response.data.data.forEach((item) => {
            this.rsus.push({

             
              nombre_entidad: item.attributes.nombre_entidad,
              pdprom: item.attributes.pdprom,
                alimentos: item.attributes.alimentos,
                calor: item.attributes.calor,
                elec_min: item.attributes.elec_min,
                elec_max: item.attributes.elec_max,
             
              provincia: item.attributes.provincia,
              municipio: item.attributes.municipio,
              latitud: item.attributes.latitud,
              longitud: item.attributes.longitud,
              tipo_residuo:item.attributes.tipo_residuo,
              
              tipo_entidad: 'Ganado Menor'
            })            
          })
        })
        .catch((error) => {
          console.error(error)
        });
        await axios
        
        .get('http://localhost:1337/api/arroceras/')
    
        .then((response) => {
          response.data.data.forEach((item) => {
            this.rsus.push({

              
              nombre_entidad: item.attributes.nombre_entidad,
              pdprom: item.attributes.pdprom,
                alimentos: item.attributes.alimentos,
                calor: item.attributes.calor,
                elec_min: item.attributes.elec_min,
                elec_max: item.attributes.elec_max,
             
              provincia: item.attributes.provincia,
              municipio: item.attributes.municipio,
              latitud: item.attributes.latitud,
              longitud: item.attributes.longitud,
              tipo_residuo:item.attributes.tipo_residuo,
              
              tipo_entidad: 'Arroceras'
            })            
          })
        })
        .catch((error) => {
          console.error(error)
        });
        await axios
        
        .get('http://localhost:1337/api/azucareras/')
    
        .then((response) => {
          response.data.data.forEach((item) => {
            this.rsus.push({

      
              nombre_entidad: item.attributes.nombre_entidad,
              pdprom: item.attributes.pdprom,
                alimentos: item.attributes.alimentos,
                calor: item.attributes.calor,
                elec_min: item.attributes.elec_min,
                elec_max: item.attributes.elec_max,
             
              provincia: item.attributes.provincia,
              municipio: item.attributes.municipio,
              latitud: item.attributes.latitud,
              longitud: item.attributes.longitud,
              tipo_residuo:item.attributes.tipo_residuo,
              
              tipo_entidad: 'Azucareras'
            })            
          })
        })
        .catch((error) => {
          console.error(error)
        });
        await axios
        
        .get('http://localhost:1337/api/pescas/')
    
        .then((response) => {
          response.data.data.forEach((item) => {
            this.rsus.push({

          
              nombre_entidad: item.attributes.nombre_entidad,
              pdprom: item.attributes.pdprom,
                alimentos: item.attributes.alimentos,
                calor: item.attributes.calor,
                elec_min: item.attributes.elec_min,
                elec_max: item.attributes.elec_max,
             
              provincia: item.attributes.provincia,
              municipio: item.attributes.municipio,
              latitud: item.attributes.latitud,
              longitud: item.attributes.longitud,
              tipo_residuo:item.attributes.tipo_residuo,
              
              tipo_entidad: 'Pesca'
            })            
          })
        })
        .catch((error) => {
          console.error(error)
        });
        await axios
        
        .get('http://localhost:1337/api/lacteas/')
    
        .then((response) => {
          response.data.data.forEach((item) => {
            this.rsus.push({

             
              nombre_entidad: item.attributes.nombre_entidad,
              pdprom: item.attributes.pdprom,
                alimentos: item.attributes.alimentos,
                calor: item.attributes.calor,
                elec_min: item.attributes.elec_min,
                elec_max: item.attributes.elec_max,
             
              provincia: item.attributes.provincia,
              municipio: item.attributes.municipio,
              latitud: item.attributes.latitud,
              longitud: item.attributes.longitud,
              tipo_residuo:item.attributes.tipo_residuo,
              
              tipo_entidad: 'Lacteas'
            })            
          })
        })
        .catch((error) => {
          console.error(error)
        });
        await axios
        
        .get('http://localhost:1337/api/cultivosvarios/')
    
        .then((response) => {
          response.data.data.forEach((item) => {
            this.rsus.push({

             
              nombre_entidad: item.attributes.nombre_entidad,
             
              pdprom: item.attributes.pdprom,
                alimentos: item.attributes.alimentos,
                calor: item.attributes.calor,
                elec_min: item.attributes.elec_min,
                elec_max: item.attributes.elec_max,
              provincia: item.attributes.provincia,
              municipio: item.attributes.municipio,
              latitud: item.attributes.latitud,
              longitud: item.attributes.longitud,
              tipo_residuo:item.attributes.tipo_residuo,
              
              tipo_entidad: 'Cultivos Varios'
            })            
          })
        })
        .catch((error) => {
          console.error(error)
        });
        await axios
        
        .get('http://localhost:1337/api/bebidaslicores/')
    
        .then((response) => {
          response.data.data.forEach((item) => {
            this.rsus.push({

            
              nombre_entidad: item.attributes.nombre_entidad,
              pdprom: item.attributes.pdprom,
                alimentos: item.attributes.alimentos,
                calor: item.attributes.calor,
                elec_min: item.attributes.elec_min,
                elec_max: item.attributes.elec_max,
             
              provincia: item.attributes.provincia,
              municipio: item.attributes.municipio,
              latitud: item.attributes.latitud,
              longitud: item.attributes.longitud,
              tipo_residuo:item.attributes.tipo_residuo,
              
              tipo_entidad: 'Bebidas Licores'
            })            
          })
        })
        .catch((error) => {
          console.error(error)
        });
        await axios
        
        .get('http://localhost:1337/api/conservas/')
    
        .then((response) => {
          response.data.data.forEach((item) => {
            this.rsus.push({

              nombre_entidad: item.attributes.nombre_entidad,
             
              pdprom: item.attributes.pdprom,
                alimentos: item.attributes.alimentos,
                calor: item.attributes.calor,
                elec_min: item.attributes.elec_min,
                elec_max: item.attributes.elec_max,
              provincia: item.attributes.provincia,
              municipio: item.attributes.municipio,
              latitud: item.attributes.latitud,
              longitud: item.attributes.longitud,
              tipo_residuo:item.attributes.tipo_residuo,
              
              tipo_entidad: 'Conserva'
            })            
          })
        })
        .catch((error) => {
          console.error(error)
        });
        await axios
        
        .get('http://localhost:1337/api/granos/')
    
        .then((response) => {
          response.data.data.forEach((item) => {
            this.rsus.push({

              nombre_entidad: item.attributes.nombre_entidad,
              pdprom: item.attributes.pdprom,
                alimentos: item.attributes.alimentos,
                calor: item.attributes.calor,
                elec_min: item.attributes.elec_min,
                elec_max: item.attributes.elec_max,
             
              provincia: item.attributes.provincia,
              municipio: item.attributes.municipio,
              latitud: item.attributes.latitud,
              longitud: item.attributes.longitud,
              tipo_residuo:item.attributes.tipo_residuo,
              
              tipo_entidad: 'Granos'
            })            
          })
        })
        .catch((error) => {
          console.error(error)
        });
        await axios
        
        .get('http://localhost:1337/api/rsus/')
    
        .then((response) => {
          response.data.data.forEach((item) => {
            this.rsus.push({

           
              nombre_entidad: item.attributes.nombre_entidad,
              pdprom: item.attributes.pdprom,
                alimentos: item.attributes.alimentos,
                calor: item.attributes.calor,
                elec_min: item.attributes.elec_min,
                elec_max: item.attributes.elec_max,
             
              provincia: item.attributes.provincia,
              municipio: item.attributes.municipio,
              latitud: item.attributes.latitud,
              longitud: item.attributes.longitud,
              tipo_residuo:item.attributes.tipo_residuo,
              
              tipo_entidad: 'RSU'
            })            
          })
        })
        .catch((error) => {
          console.error(error)
        });
      },
      
      makeRequest(value) {
          console.log(
            `input:`,
            value
          );
        },
  
      
  
     
  
     
  
  
      },
    }
  
  
  </script>
  