<script setup>
import { ref } from 'vue'
import { cercarPerText, detallPelicula } from '../services/CommunicationManager.js'

const textCerca = ref('')
const resultats = ref([])
const mostrarInfo = ref(false)
const infoPelicula = ref(null)
const cargant = ref(false)

async function cercar() {
  resultats.value = await cercarPerText(textCerca.value)
  console.log(resultats.value)
}

async function Info(imdbID) {

  cargant.value = true           
  infoPelicula.value = null
  mostrarInfo.value = true

      try {
        const detalls = await detallPelicula(imdbID)
        infoPelicula.value = detalls
      } catch (err) {
        console.error(err)

      }finally{
        cargant.value = false
      }
}

async function borrar() {
  textCerca.value = ''
  resultats.value = []
}

</script>

<template>
  <v-container>
    <v-text-field
      v-model="textCerca"
      label="Què vols cercar?"
      append-inner-icon="mdi-magnify"
      @keydown.enter="cercar"
      
    ></v-text-field>

    <v-btn
              @click="borrar"
              color="primary">            
              Netejar el cercador
            </v-btn>
    <br><br>
    <v-btn @click="cercar">Cercar</v-btn>

    

    
    <br>
    <div v-if="resultats.length==0">
      No s'han trobat resultats
    </div>

      <div>S'han trobat {{ resultats.length }} pel·licules </div>
      <br>
    <v-row>
      <v-col v-for="element in resultats" :key="element.imdbID" cols="12" sm="6" md="4">
        <v-card>
          <v-img :src="element.Poster" height="350" cover></v-img>

          <v-card-title>{{ element.Title }}</v-card-title>

          <v-card-text>
            {{ element.Year }} 
          </v-card-text>

          <v-card-text>
            {{ element.Type }}
          </v-card-text>

          <v-card-actions>
            <v-btn
              @click="Info(element.imdbID)"
              color="primary">            
              Més info
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>

  <v-dialog v-model="mostrarInfo" max-width="500">
    <v-card>
          <div v-if="cargant">
            <v-progress-circular 
              color="blue-lighten-3" 
              indeterminate 
              :size="49" 
              :width="6">
            </v-progress-circular>
    </div>

      <v-card-text v-if="infoPelicula">
        
        <p><b>Escritor : </b>{{ infoPelicula.Writer }}</p>

        <p><b>Gènere : </b>{{ infoPelicula.Genre }}</p>

        <p><b>Director : </b>{{ infoPelicula.Director }}</p>
        <p><b>Actors : </b>{{ infoPelicula.Actors }}</p>

        <p><b>Plot : </b>{{ infoPelicula.Plot }}</p>

        <p><b>Durada : </b>{{ infoPelicula.Runtime }}</p>
        <p><b>Any : </b>{{ infoPelicula.Year }}</p>
      </v-card-text>

    </v-card>

  </v-dialog>



</template>
