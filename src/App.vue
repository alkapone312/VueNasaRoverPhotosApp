<template>
  <div class="wrapper">

    <HeroImage />

    <Transition name="view">
      <Picker v-if="this.ready && this.mode === 0" @request="handleRequest"/>
    </Transition>

    <Transition name="noResults">
      <NoResults v-if="this.noResults"/>
    </Transition>


  </div>
</template>

<script>
import axios from 'axios';

import HeroImage from './components/HeroImage.vue';
import Picker from './components/Picker.vue';
import NoResults from './components/NoResults.vue';
//create your own api key here - https://api.nasa.gov/
import Config from './config.js';

const API = 'https://api.nasa.gov/mars-photos/api/v1/rovers';
const api_key = Config.api_key;

export default {

  name: 'App',

  data () {
    return {
      noResults: false,
      ready: false,
      page:1,
      mode: 0, // 0 - search, 1 - browse picks
      data: null,
    }
  },

  methods: {
    handleRequest (request) {
      console.log(request);
      let queryString = '';

      request.day = ('00'+ request.day).slice(-2);
      request.month = ('00'+ request.month).slice(-2);

      if(request.camera !== '')
        queryString+=`camera=${request.camera}&`;

      if(request.dateType)
        queryString+=`earth_date=${request.year}-${request.month}-${request.day}&`;

      if(!request.dateType)
        queryString+=`sol=${request.sol}&`;

      queryString+=`page=${this.page}`;

      //console.log(queryString);
      axios.get(`${API}/${request.rover}/photos?api_key=${api_key}&${queryString}`)
            .then((response)=>{
              console.log(response);
              this.data = response.data;
              if(this.data.photos.length == 0)
              {
                this.noResults = true;
                setTimeout(()=>{this.noResults = false}, 3000);
              }
              else
                this.mode = 1;
            })
            .catch((error) => {
              console.log(error);
            });
    }
  },

  components: {
    NoResults,
    Picker,
    HeroImage,
  },

  mounted() {
    setTimeout(()=>{this.ready = true},100);
  }
}
</script>

<style lang="scss">
 @import url('https://fonts.googleapis.com/css2?family=Maven+Pro:wght@400;600');

  *
  {
    box-sizing: border-box;
  }

  body
  {
    margin: 0;
    padding: 0;
    font-family: 'Maven Pro', sans-serif;
    overflow: hidden;
  }

  input[type="number"] {
  -webkit-appearance: textfield;
     -moz-appearance: textfield;
          appearance: textfield;
  }
  input[type=number]::-webkit-inner-spin-button, 
  input[type=number]::-webkit-outer-spin-button { 
    -webkit-appearance: none;
  }

  .view-enter-active,
  .view-leave-active {
    transition: all 1s ease;
    transition-delay: .1s;
  }

  .view-enter-from,
  .view-leave-to {
    opacity: 0;
    margin-top: 50px;
  }

  .noResults-enter-active,
  .noResults-leave-active {
    transition: all 1s ease;
    transition-delay: .1s;
  }

  .noResults-enter-from,
  .noResults-leave-to {
    opacity: 0;
    margin-top: -60px;
  }
</style>