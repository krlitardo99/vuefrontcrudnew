<template>
<div>
    <h2>Espacio de busqueda</h2>
    <form @submit.prevent="enviarFormulario">
      <div>
        <label>Nombre:</label>
        <input type="text" v-model="formulario.nombre" required>
      </div>
      <div>
        <label>Cedula:</label>
        <input type="int" v-model="formulario.especialidad" required>
      </div>
      <div>
        <label>Correo Electronico:</label>
        <input type="text" v-model="formulario.sede" required>
      </div>
      <div>
        <label>Sede:</label>
        <input type="text" v-model="formulario.doctor" required>
      </div>
      
      <!-- Agrega más campos del formulario aquí -->
      <button type="submit">Enviar</button>
    </form>
  </div>

</template>


<script>
import axios from 'axios';

export default {
    data() {
        return {
            formulario: {
                nombre:'',
            }
        }
    },
    methods: {
    enviarFormulario() {
      axios.post('http://127.0.0.1:8000/app/api/c/', this.formulario)
        .then(response => {
          console.log('Datos enviados correctamente:', response.data);
          // Aquí puedes manejar la respuesta del servidor si es necesario
        })
        .catch(error => {
          console.error('Error al enviar datos:', error);
        });
    }
  }
};
</script>