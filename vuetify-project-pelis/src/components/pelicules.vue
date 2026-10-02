<script setup>
import { ref } from 'vue'
import { cercarPerText } from '../services/CommunicationManager.js'

const textCerca = ref('')
const resultats = ref([])

async function cercar() {
  resultats.value = await cercarPerText(textCerca.value)
  console.log(resultats.value)
}
</script>

<template>
  <v-container>
    <v-text-field
      v-model="textCerca"
      label="Què vols cercar?"
    ></v-text-field>

    <v-btn @click="cercar">Cercar</v-btn>
    

    <v-row>
      <v-col
        v-for="element in resultats"
        :key="element.imdbID"
        cols="12"
        md="4"
      >
        <v-card>
          <v-img :src="element.Poster" height="350" cover></v-img>

          <v-card-title>{{ element.Title }}</v-card-title>

          <v-card-text>
            {{ element.Year }} · {{ element.Type }}
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>