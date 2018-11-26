<template>
  <div class="border-page">
    <h1 v-show="isLoading">
      Cargando personajes ...
    </h1>
    <div v-show="!isLoading">
      <h1 class="center">Listado de Personajes</h1>
      <table >
        <tr >
          <td class="bg-title" height="25">Nombre</td>
          <td class="bg-title" height="25">Casa</td>
          <td class="bg-title" height="25">Detalle</td>
        </tr>
        <tr v-for="character in characters">
          <td>{{ character.name }}</td>
          <td>{{ character.house }}</td>
          <td> <button @click="goToDetail(character._id)">Ver detalle</button> </td>
        </tr>
      </table>
    </div>
  </div>
</template>


<script>
  import { listsAllCharacters } from '../services/got.service.js'
  import { router } from '../main'

  export default {
    name: 'list-component',

    /**
     * @description the data block represents all the local variable of this component.
     */
    data () {
      return {
        characters: [],
        isLoading: false
      }
    },

    /**
     * @description the create function is the first one to be execute when the component is being created (see vue js lifecycle).
     */
    created () {
      this.isLoading = true
      listsAllCharacters()
        .then(res => {
          this.characters = res
          this.isLoading = false
        })
    },

    /**
     * @description the methods block represents all the local methods of this component.
     */
    methods: {

      /**
       * @description get the detail of a character from the GoT API.
       * @param {string} id. the "_id" of the character that we are going to request.
       * @method goToDetail
       */
      goToDetail(id) {
        router.push({ name: "detail", params: { id: id } });
      }
    }
  }
</script>
<style>
  body {
    font-family: Arial, Helvetica, sans-serif;
    font-weight: 200;
    color: #002642;
  }

  .border-page {
    border: 2px solid #dddddd;
    margin: 10px;
    padding: 10px 15px;
  }

  .center {
    text-align: center;
  }

  .bg-title {
    background-color: #002642;
    color: #FDFFFC;
    padding: 2px 0px;
    text-align: center;
  }

  table {
    font-family: arial, sans-serif;
    border-collapse: collapse;
    width: 100%;
  }

  td {
    border: 1px solid #dddddd;
    text-align: left;
    padding: 8px;
    font-size: 14pt;
  }

  tr:nth-child(even) {
    background-color: #dddddd;
  }
</style>
