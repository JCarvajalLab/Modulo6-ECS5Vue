<template>
  <div class="container">
    <button class="btn-ingresar" @click="ingresarDatos">Ingresar Datos</button>
    <div v-if="nombre" class="welcome-message">Bienvenid@ {{ nombre }} {{ apellido }}</div>
    <div class="info-text" v-if="nombre">Aquí podrás encontrar información de películas de anime de Studio Ghibli</div>
    <button class="btn-limpiar" v-if="nombre" @click="limpiar">Limpiar información</button>

    <DetalleEstudios v-if="nombre" @peliculasLoaded="setPeliculas" />
  </div>
</template>

<script>
import DetalleEstudios from '@/components/DetalleEstudios.vue';

export default {
  name: 'PaginaPrincipal',
  components: {
    DetalleEstudios
  },
  data() {
    return {
      nombre: '',
      apellido: '',
      peliculas: [],
      selectedPelicula: null,
      detallesPelicula: null
    };
  },
  methods: {
    ingresarDatos() {
      this.nombre = prompt("Por favor, ingresa tu nombre:");
      this.apellido = prompt("Por favor, ingresa tu apellido:");
    },
    limpiar() {
      this.nombre = '';
      this.apellido = '';
      this.selectedPelicula = null;
      this.detallesPelicula = null;
    },
    setPeliculas(peliculas) {
      this.peliculas = peliculas;
    },
    mostrarDetalles() {
      if (this.selectedPelicula) {
        // Asumiendo que la película tiene las propiedades necesarias
        this.detallesPelicula = {
          romanized_title: this.selectedPelicula.romanized_title || 'N/A',
          english_title: this.selectedPelicula.english_title || 'N/A',
          image: this.selectedPelicula.image || 'default_image.jpg',
          director: this.selectedPelicula.director || 'N/A',
          description: this.selectedPelicula.description || 'N/A',
          duration: this.selectedPelicula.duration || 'N/A'
        };
      } else {
        this.detallesPelicula = null;
      }
    }
  }
}
</script>

<style>
/* Aquí puedes agregar estilos para tus botones y otros elementos */
body {
    font-family: Arial, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
    background-color: #f5f5f5;
}
.container {
    text-align: center;
    background-color: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}
.btn-ingresar {
    background-color: #28a745;
    color: white;
    border: none;
    padding: 10px 20px;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
    margin-bottom: 20px;
}
.btn-ingresar:hover {
    background-color: #218838;
}
.welcome-message {
    font-size: 24px;
    font-weight: bold;
    color: #333;
}
.info-text {
    color: #666;
    margin: 10px 0 20px 0;
}
.btn-limpiar {
    background-color: #ffc107;
    color: black;
    border: none;
    padding: 10px 20px;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
}
.btn-limpiar:hover {
    background-color: #e0a800;
}
.peliculas-list {
    margin-top: 20px;
}

</style>