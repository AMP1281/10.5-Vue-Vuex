<template>

<div>

    <form>

      <input class="w-100 my-3" v-model="titleInput" placeholder="Buscar..." type="text" name="title">
      {{titleInput}}

      <div class="row my-3">

          <div class="col">
                <div class="form-check text-end">
                    <input class="form-check-input" type="radio" name="gridRadios" id="gridRadios1" value="optionAvailable" v-model="x">
                    <label class="form-check-label" for="gridRadios1">Disponible</label>
                </div>
          </div>

          <div class="col">
                  <div class="form-check">
                      <input class="form-check-input" type="radio" name="gridRadios" id="gridRadios2" value="optionNotAvailable" v-model="x">
                      <label class="form-check-label" for="gridRadios2">No Disponible</label>
                  </div>
          </div>

      </div>

    </form>

    <div>

      <div class="container">

                <ul class="row list-unstyled">

                    <li class="col-lg-4 mt-4 d-flex align-items-stretch justify-content-center" 
                    v-for="(item, index) in resultados" :key="index">

                            <div class="card text-center w-100">
                              <h5 class="card-header py-4">
                                {{item.title}} <br>
                                Available: {{item.available}}</h5>

                              <div class="card-body py-4">

                                <p class="card-text">{{item.description}}</p>

                              </div>
                      </div>
                        
                  </li>
              </ul>

        <div v-if="resultados.length<1">
              <p class="alert alert-danger">No hay películas disponibles</p>
        </div>

    </div>

    </div>

</div>

</template>

<script>
import {mapState, mapGetters, mapMutations} from 'vuex'

export default {
  data(){
    return{
      titleInput:'',
      x:'optionAvailable',
    }
  },

  computed:{
    ...mapState(['movies']),
    ...mapGetters(['availableTrueMovies','availableFalseMovies', 'getMoviesAvailable','getMoviesNotAvailable']),

    //Busca por título en la lista de disponibles
    buscarAvailable(){
         return this.$store.getters['getMoviesAvailable'](this.titleInput);
    },

    //Busca por título en la lista de NO disponibles
    buscarNotAvailable(){
         return this.$store.getters['getMoviesNotAvailable'](this.titleInput);
    },



    resultados(){
      if (this.titleInput ==="" && this.x === 'optionAvailable'){
        return this.availableTrueMovies;
      }

      if (this.titleInput ==="" && this.x === 'optionNotAvailable'){
        return this.availableFalseMovies
      }

      if(this.titleInput.length > 2 && this.titleInput !=="" && this.x === 'optionAvailable'){
        return this.buscarAvailable
      }

      if(this.titleInput.length > 2 && this.titleInput !=="" && this.x === 'optionNotAvailable'){
        return this.buscarNotAvailable
      }

      else{
        return false
      }

    },

  },

  methods:{
    ...mapMutations(['crearArray']),
  },

}


</script>


<style scoped>


</style>
