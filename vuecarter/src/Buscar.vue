<template>
    <div>
      <h2>Espacio de búsqueda</h2>
      <div>
        <label>Nombre:</label>
        <input type="text" v-model="formulario.nombre" required>
        <button @click="buscarResultados">Buscar</button>
      </div>
      <table v-if="resultadosFiltrados.length > 0">
        <thead>
          <tr>
            <th>Nombre</th>
            <th>Especialidad</th>
            <th>Correo Electrónico</th>
            <th>Sede</th>
            <th>Acciones</th> <!-- Nueva columna para acciones -->
          </tr>
        </thead>
        <tbody>
          <tr v-for="resultado in resultadosFiltrados" :key="resultado.id">
            <td>{{ resultado.name }}</td>
            <td>{{ resultado.especiality }}</td>
            <td>{{ resultado.doctor }}</td>
            <td>{{ resultado.sede }}</td>
            <td>
              <button @click="editarResultado(resultado)" class="btn-editar">Editar</button>
              <button @click="eliminarResultado(resultado.id)" class="btn-eliminar">Eliminar</button>
            </td>
          </tr>
        </tbody>
      </table>
      <p v-else>No se encontraron resultados.</p>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        formulario: {
          nombre: '',
        },
        resultados: []
      }
    },
    computed: {
      resultadosFiltrados() {
        return this.resultados.filter(resultado => resultado.name.toLowerCase().includes(this.formulario.nombre.toLowerCase()));
      }
    },
    methods: {
      buscarResultados() {
        axios.get(`http://127.0.0.1:8000/app/api/c/`, {
          params: {
            nombre: this.formulario.nombre
          }
        })
          .then(response => {
            this.resultados = response.data;
            console.log('Resultados:', response.data);
          })
          .catch(error => {
            console.error('Error al buscar resultados:', error.message);
          });
      },
      editarResultado(resultado) {
        // Implementa la lógica para editar el resultado
        console.log('Editar resultado:', resultado);
      },
      eliminarResultado(id) {
  axios.delete(`http://127.0.0.1:8000/app/api/c/${id}`)
    .then(response => {
      // Aquí puedes actualizar la lista de resultados después de eliminar
      console.log('Resultado eliminado con éxito:', response.data);
      // Por ejemplo, podrías volver a cargar los resultados después de eliminar uno
      this.buscarResultados();
    })
    .catch(error => {
      console.error('Error al eliminar resultado:', error.message);
    });
}

    }
  };
  </script>
  
  <style scoped>
  .btn-editar,
  .btn-eliminar {
    margin-right: 5px;
  }
  </style>