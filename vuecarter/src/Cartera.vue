<template>
    <div>
      <input type="text" v-model="searchQuery" @input="filterData" placeholder="Buscar...">
      <table>
        <thead>
          <tr>
            <th>Nombre</th>
            <th>Especialidad</th>
            <th>Médico</th>
            <th>Sede</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="record in filteredData" :key="record.id">
            <td>{{ record.nombre }}</td>
            <td>{{ record.especialidad }}</td>
            <td>{{ record.medico }}</td>
            <td>{{ record.sede }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        searchQuery: '',
        dataFromDjango: []
      };
    },
    created() {
      this.fetchDataFromDjango();
    },
    computed: {
      filteredData() {
        return this.dataFromDjango.filter(record =>
          record.nombre.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
          record.especialidad.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
          record.medico.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
          record.sede.toLowerCase().includes(this.searchQuery.toLowerCase())
        );
      }
    },
    methods: {
      async fetchDataFromDjango() {
        try {
          const response = await axios.get('http://127.0.0.1:8000/app/api/c/');
          this.dataFromDjango = response.data;
        } catch (error) {
          console.error('Error al obtener los datos de Django:', error);
        }
      },
      filterData() {
        // No necesitas hacer nada aquí, el cambio en la propiedad 'searchQuery' automáticamente filtrará los datos.
      }
    }
  };
  </script>
  