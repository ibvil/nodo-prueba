<template>
  <div class="border-page">
    <h1 v-show="isLoading">
      Cargando detalles del personaje ...
    </h1>

    <div v-show="!isLoading">
      <h1 class="center">{{character.name}}</h1>
      <table cellspacing="0" cellpadding="0" width="100%">
          <tr>
            <td width="40%" valign="top" class="center">
                <table cellspacing="0" cellpadding="15" width="100%" class="text">
                    <tr>
                        <td colspan="2" class="bg-title" height="25">
                            Imagen Personaje
                        </td>
                    </tr>
                    <div v-if="image">
                      <img v-bind:src="image" height='400'>
                    </div>
                    <div v-else>
                      Sin imagen disponible
                    </div>
                </table>
            </td>
            <td width="1%"></td>
              <td width="59%" valign="top">
                  <table cellspacing="0" cellpadding="0" width="100%" class="text">
                      <tr>
                          <td colspan="2" class="bg-title" height="25">
                              Información Personaje
                          </td>
                      </tr>
                      <tr>
                          <td colspan="2"></td>
                      </tr>
                      <tr>
                          <td width="200" valign="top">Nombre</td>
                          <td valign="top">{{character.name}}</td>
                      </tr>
                      <tr>
                          <td valign="top">Fecha de Nacimiento</td>
                          <td valign="top"><span v-if="character.dateOfBirth">{{character.dateOfBirth}}</span><span v-else>Sin información</span></td>
                      </tr>
                      <tr>
                          <td valign="top">Fecha de Fallecimiento</td>
                          <td valign="top"><span v-if="character.dateOfDeath">{{character.dateOfDeath}}</span><span v-else>Sin información</span></td>
                      </tr>
                      <tr>
                          <td valign="top">Sexo</td>
                          <td valign="top"><span v-if="character.male">Masculino</span> <span v-else>Femenino</span></td>
                      </tr>
                      <tr>
                          <td valign="top">Cultura</td>
                          <td valign="top"><span v-if="character.culture">{{character.culture}}</span><span v-else>Sin información</span></td>
                      </tr>
                      <tr>
                          <td valign="top">Casa</td>
                          <td valign="top"><span v-if="character.house">{{character.house}}</span><span v-else>Sin información</span></td>
                      </tr>
                      <tr >
                          <td valign="top">Títulos</td>
                          <td valign="top"><span v-for="(title, index) in character.titles">{{index+1}} - {{ title }}<br></span></td>
                      </tr>
                      <tr>
                          <td valign="top">Libros</td>
                          <td valign="top"><span v-for="(book, index) in character.books">{{index+1}} - {{book}}<br></span></td>
                      </tr>
                  </table>
              </td>
          </tr>
      </table>

      <table v-show="gotHouse" cellspacing="0" cellpadding="0" width="100%">
          <tr>
              <td width="59%" valign="top">
                  <table cellspacing="0" cellpadding="0" width="100%" class="text">
                      <tr>
                          <td colspan="2" class="bg-title" height="25">
                              Detalles {{character.house}}
                          </td>
                      </tr>
                      <tr>
                          <td colspan="2"></td>
                      </tr>
                      <tr>
                          <td width="200" valign="top">Lema</td>
                          <td valign="top">{{house.words}}</td>
                      </tr>
                      <tr>
                          <td valign="top">Título</td>
                          <td valign="top"><span v-if="house.title">{{house.title}}</span><span v-else>Sin información</span></td>
                      </tr>
                      <tr>
                          <td valign="top">Región</td>
                          <td valign="top"><span v-if="house.region">{{house.region}}</span><span v-else>Sin información</span></td>
                      </tr>
                      <tr>
                          <td valign="top">Escudo de Armas</td>
                          <td valign="top"><span v-if="house.coatOfArms">{{house.coatOfArms}}</span> <span v-else>Sin información</span></td>
                      </tr>
                      <tr>
                          <td valign="top">Líder Actual</td>
                          <td valign="top"><span v-if="house.currentLord != 'unknown'">{{house.currentLord}}</span><span v-else>Sin información</span></td>
                      </tr>
                      <tr>
                          <td valign="top">Casa Mandante</td>
                          <td valign="top"><span v-if="house.overlord">{{house.overlord}}</span><span v-else>Sin información</span></td>
                      </tr>
                      <tr >
                          <td valign="top">Arma Ancestral</td>
                          <td valign="top"><span v-for="(weapon, index) in house.ancestralWeapon">{{index+1}} - {{ weapon }}<br></span></td>
                      </tr>
                  </table>
              </td>
              <td width="1%"></td>
              <td width="40%" valign="top" class="center">
                  <table cellspacing="0" cellpadding="10" width="100%" class="text">
                      <tr>
                          <td colspan="2" class="bg-title" height="25">
                            Escudo {{character.house}}
                          </td>
                      </tr>
                      <div v-if="houseImage">
                        <img v-bind:src="houseImage" height='400'>
                      </div>
                      <div v-else>
                        Sin imagen disponible
                      </div>
                  </table>
              </td>
          </tr>
      </table>

      <br>
      <div class="center">
        <button @click="goBack()">Volver a listado</button>
      </div>

    </div>
  </div>

</template>

<script>
  import { getACharacter, getHouse } from '../services/got.service.js'
  import { router } from '../main'

  export default {
    name: 'detail-component',

    /**
     * @description the data block represents all the local variable of this component.
     */
    data () {
      return {
        character: {},
        house: {},
        image: '',
        houseImage: '',
        isLoading: false,
        gotHouse: false
      }
    },

    /**
     * @description the create function is the first one to be execute when the component is being created (see vue js lifecycle).
     */
    created () {
      this.isLoading = true
      getACharacter(this.$route.params.id)
        .then(res => {
          this.character = res
          this.image = res.imageLink ? 'https://api.got.show'+res.imageLink : null ;
          this.isLoading = false
          if (res.house) {
            getHouse(res.house).then( res => {
              this.gotHouse = res.name ? true : false
              this.house = res

              this.houseImage = res.imageLink ? 'https://api.got.show'+res.imageLink : null ;
            })
          }
        })
    },

    /**
     * @description the methods block represents all the local methods of this component.
     */
    methods: {
      /**
       * @description Vuelve al listado de personajes
       * @method goBack
       */
      goBack() {
        router.go(-1);
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
    border: 2px solid #002642;
    margin: 10px;
    padding: 10px 15px;
  }

  .center {
    text-align: center;
  }

  .bg-title {
    background-color: #002642;
    color: #FDFFFC;
    padding: 4px 0px;
    text-align: center;
    font-size: 18px;
  }

  table {
    border-collapse: collapse;
    width: 100%;
  }

  td {
    border: 1px solid #a0a0a0;
    text-align: left;
    padding: 8px;
    font-size: 16px;
  }

  tr:nth-child(even) {
    background-color: #dddddd;
  }

</style>
