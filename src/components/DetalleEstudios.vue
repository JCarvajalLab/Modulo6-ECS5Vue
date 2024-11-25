<template>
  <div>
    <select v-model="selectedPelicula" @change="mostrarDetalles">
      <option v-for="pelicula in peliculas" :key="pelicula.id" :value="pelicula">
        {{ pelicula.title }}
      </option>
    </select>

    <div v-if="detallesPelicula" class="detalles-pelicula">
      <img :src="detallesPelicula.image" alt="Imagen de la película" class="pelicula-imagen" />
      <h2><strong>Titulo:</strong> {{ detallesPelicula.original_title_romanised }}</h2>
      <h3><strong>Titulo ingles:</strong> {{ selectedPelicula.title }}</h3>
      <p style="text-align:justify"><strong>Descripción:</strong> {{ detallesPelicula.description }}</p>
      <p><strong>Director:</strong> {{ selectedPelicula.director }}</p>
      <p><strong>Duración:</strong> {{ detallesPelicula.duration }} minutos</p>

    <div v-if="selectedPelicula" class="pelicula-info">
      
      
    </div>
    </div>

  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'DetalleEstudios',
  data() {
    return {
      peliculas: [], // Para almacenar las películas
      selectedPelicula: null, // Para almacenar la película seleccionada
      detallesPelicula: null // Para almacenar los detalles de la película seleccionada
    };
  },
  created() {
    this.cargarPeliculas(); // Llama a la API cuando el componente se crea
  },
  methods: {
    cargarPeliculas() {
      axios.get('https://ghibliapi.vercel.app/films')
        .then(response => {
          this.peliculas = response.data; // Almacena la lista de películas en el data
        })
        .catch(error => {
          console.error("Error al obtener las películas:", error);
        });
    },
    mostrarDetalles() {
      if (this.selectedPelicula) {
        this.detallesPelicula = {
          original_title_romanised: this.selectedPelicula.original_title_romanised,
          title: this.selectedPelicula.title,
          director: this.selectedPelicula.director,
          description: this.selectedPelicula.description,
          duration: this.selectedPelicula.duration,
          image: this.selectedPelicula.image
        };
      } else {
        this.detallesPelicula = null; // Resetea los detalles si no hay selección
      }
    }
  }
}
</script>

<style>
.pelicula-info {
  font-size: 20px; /* Tamaño de fuente para la información básica */
  margin-bottom: 10px; /* Espacio entre la información básica y los detalles */
}

.detalles-pelicula {
  font-size: 14px; /* Tamaño de fuente más pequeño para los detalles */
  background-color: #f9f9f9; /* Fondo claro para diferenciar */
  padding: 10px; /* Espaciado interno */
  border-radius: 5px; /* Bordes redondeados */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Sombra sutil */
  margin: 20px auto; /* Margen superior e inferior, centrado horizontalmente */
  width: 300px; /* Ancho fijo para la sección de detalles */
 box-shadow: 2px 2px 2px 2px rgba(0, 0, 0, 0.2);
}

.pelicula-imagen {
  max-width: 100%; /* Asegúrate de que la imagen no exceda el ancho del contenedor */
  height: 400px; /* Mantiene la proporción de la imagen */
  
}
</style>
