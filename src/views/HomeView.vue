<!-- html -->
<template>
  <div>
    <div class="center">
      <!--Sin el div por no funcionaba por que todo debe estar dentro de un div-->
      <img src="../cab_pokedex.jpg">
    </div>
    <div class="flex">
      <div class="flex-col" style="flex: 0 0 3%">
        <div class="content">
          <div v-for="(data, index) in pokemons" :key="index">
            {{ i }}
            <v-app id="inspire" class="size">
              <v-card class="mx-auto" max-width="344">
                <!--Los dos puntos los toma como una variable-->
                <v-img class="pointer" :src="data.url" height="200px" @click="send_info(data)"></v-img>

                <!-- <v-card-title v-if=" i < 3"> menor que 3 </v-card-title> -->
                <!-- al else no se le pone condicion-->
                <!-- <v-card-title v-else> mayor que 3 </v-card-title> -->
                <v-card-title> {{ data.name }} </v-card-title>
                <v-card-subtitle>
                  1,000 miles of wonder
                </v-card-subtitle>

                <v-card-actions>
                  <v-btn color="orange lighten-2" text>
                    Explore
                  </v-btn>

                  <v-spacer></v-spacer>

                  <v-btn icon @click="show = !show">
                    <v-icon>{{ show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
                  </v-btn>
                </v-card-actions>

                <v-expand-transition>
                  <div v-show="show">
                    <v-divider></v-divider>

                    <v-card-text>
                      I'm a thing. But, like most politicians, he promised more than he could deliver. You won't have
                      time
                      for
                      sleeping, soldier, not with all the bed making you'll be doing. Then we'll go with that data file!
                      Hey,
                      you add a one and two zeros to that or we walk! You're going to do his laundry? I've got to find a
                      way
                      to escape.
                    </v-card-text>
                  </div>
                </v-expand-transition>
              </v-card>
            </v-app>
          </div>
        </div>
      </div>
      <div class="flex-col">
        <!--Recibimos el send url y recieve_url es para hacer un metodo-->
        <!--No poner parentesis en el metodo -->
        <Pokemoninfo :pokemoninfo="selected_pokemon" @send_url="receive_url"></Pokemoninfo>
      </div>
    </div>
  </div>
</template>



<!-- variables -->
<script>
//Importar axios el comentario de abajo fue necesario por un error de axios y eslint
//tiene que ir sin salto de linea entre el comentario y el impor para que funcione
// eslint-disable-next-line no-unused-vars
import axios from "axios";
/// Ojo en algun momento de alguna forma se instal un import que nunca se debio instalar solo
// de express
import Pokemoninfo from "../components/Pokemoninfo.vue";


export default {
    component: {Pokemoninfo},
    data() {
        return {
            show: false,
            isActive: true,
            pokemons: [],
            selected_pokemon: []
        };
    },
    // Se ejecuta primero, cuando todavia no hay nada visible - 
    //Aqui va la data exitente ej Apis
    created() {
        //solo dentro de axios necesitamos la instancia por que this no funciona
        let instance = this;
        for (let i = 0; i <= 10; i++) {
            axios
                .get(`https://pokeapi.co/api/v2/pokemon/${i + 1}`)
                .then((res) => {
                let pokemon = {
                    name: res.data.name,
                    url: res.data.sprites.front_default,
                    abilities: res.data.abilities,
                };
                instance.pokemons.push(pokemon);
            })
                .catch((err) => {
                console.log(err);
            });
            console.log(this.pokemons);
        }
    },
    // Se ejecuta despues del renderizado de las cosas
    // Cuando cambia la data en una variable
    mounted() {
    },
    methods: {
        send_info(Pokemoninfo) {
            this.selected_pokemon = Pokemoninfo.abilities
        },
        // enviado desde el send url... pasa por @send url y llega hasta aca...
        receive_url(url){
        window.location.replace(url)        
}
    },
    components: { Pokemoninfo }
}
</script>


<!-- estilos -->
<!-- la palabra scope significa que solo sera para esta vista -->
<style scoped>
.center {
  text-align: center;
  margin-top: 1%;
}

.size {
  height: 400px;

}

.pointer {
  cursor: pointer;


}

.flex {
  display: flex;
}

.flex-col {
  flex: 2;
  background-color: lightblue;
}
</style>
