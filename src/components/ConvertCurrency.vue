<template>
  <v-container>
    <v-card
    class="mx-auto mt-5"
    width="500"
    >
      <template v-slot:title>
        <v-col cols="12">
          <v-img
            :src="logo"
            class="my-3"
            contain
            height="100"
          />
        </v-col>
        <span class="font-weight-black">Piedra, papel o tijera</span>
      </template>

      <v-card-text class="bg-surface-light pt-4 text-center">
        <v-row>
          <v-col cols="12" md="12">
            <v-btn 
            v-if="!intervalId"
            class="text-none text-subtitle-1 mb-2"
            color="#5865f2"
            size="small"
            variant="flat" @click="startImageRotation()">Jugar de nuevo</v-btn>
          </v-col>
        </v-row>
        <v-divider></v-divider>
            <div class="text-caption">Elegir</div>
        <v-divider class="mb-2"></v-divider>
        <v-row>
          <v-col cols="12" md="4">
            <v-btn @click="selectImage('rock')">Piedra</v-btn>
          </v-col>
          <v-col cols="12" md="4">
            <v-btn @click="selectImage('paper')">Papel</v-btn>
          </v-col>
          <v-col cols="12" md="4">
            <v-btn @click="selectImage('scissors')">Tijera</v-btn>
          </v-col>
        </v-row>
        <v-row justify="center">
          <v-img :src="currentImageOne" max-width="200" />
        </v-row>

        <v-row justify="center">
          <h1>V.S</h1>
        </v-row>

        <v-row justify="center">
          <v-img :src="currentImageTwo" max-width="200" />
        </v-row>

        <v-row v-if="result">
          <v-col cols="12" md="12">
            <v-alert v-if="result" type="success">
              {{ result }}
            </v-alert>
          </v-col>
        </v-row>

      </v-card-text>
      <v-card-text>
        <div class="text-medium-emphasis text-justify">
          Proyecto N3: Este proyecto utiliza Vue.js para construir la interfaz de usuario, Vuetify para los componentes estilizados. Es un juego interactivo de 'Piedra, Papel o Tijera', donde los usuarios pueden competir contra la computadora.
        </div>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script>
import axios from 'axios';
import Swal from 'sweetalert2';
import logo from '../assets/javier-logo-color.png'
import rockImg from '../assets/rock.png';
import paperImg from '../assets/paper.png';
import scissorsImg from '../assets/scissors.png';

export default {
  name: 'HelloWorld',

  data: () => ({
    logo,
    playerChoice: '',
    computerChoice: '',
    result: '',
    images: {
        rock: rockImg,
        paper: paperImg,
        scissors: scissorsImg
      },
    currentImageOne: '',
    currentImageTwo: '',
    selectedImage: null,
    intervalId: null
  }),
  mounted() {
    this.startImageRotation();
  },
  beforeDestroy() {
    clearInterval(this.intervalId);
  },
  methods: {
    playGame(playerChoice) {
      this.playerChoice = playerChoice;
      this.result = this.calculateResult();
      this.showSuccessAlert();
    },
    calculateResult() {
      if (this.playerChoice === this.computerChoice) {
        return 'Empate';
      } else if (
        (this.playerChoice === 'rock' && this.computerChoice === 'scissors') ||
        (this.playerChoice === 'paper' && this.computerChoice === 'rock') ||
        (this.playerChoice === 'scissors' && this.computerChoice === 'paper')
      ) {
        return '¡Ganaste!';
      } else {
        return 'Perdiste';
      }
    },
    startImageRotation() {
      const imageKeys = Object.keys(this.images);
      let currentIndexOne = 0;
      let currentIndexTwo = 0;
      
      this.intervalId = setInterval(() => {
        this.computerChoice = imageKeys[currentIndexTwo];
        this.currentImageOne = this.images[imageKeys[currentIndexOne]];
        this.currentImageTwo = this.images[imageKeys[currentIndexTwo]];
        currentIndexOne = Math.floor(Math.random() * imageKeys.length);
        currentIndexTwo = Math.floor(Math.random() * imageKeys.length);
      }, 150);
    },
    selectImage(imageType) {
      clearInterval(this.intervalId);
      this.intervalId = null;
      this.currentImageOne = this.images[imageType];
      this.playGame(imageType);
    },
    showSuccessAlert() {
      Swal.fire({
        title: `¡¡¡  ${this.result} !!!`,
        text: 'Resultado',
        icon: 'success',
        background: '#f9f9f9',
        showConfirmButton: false,
        toast: true,
        timer: 4000,
        timerProgressBar: true,
        position: 'top-end',
      });
    },
    showErrorAlert() {
      Swal.fire({
        title: '¡Error!',
        text: 'Ocurrió un error al obtener la tasa de cambio.',
        icon: 'error',
      });
    },
    showValidationAlert() {
      Swal.fire({
        title: '¡Campos Incompletos!',
        text: 'Por favor, completar todos los campos.',
        icon: 'warning'
      });
    }
  },
}
</script>
