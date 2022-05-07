<template>
  <div class="wrapper">
    <HeroImage />
    <Picker @request="handleRequest"/>
  </div>
</template>

<script>
import axios from 'axios';

import HeroImage from './components/HeroImage.vue';
import Picker from './components/Picker.vue';
//create your own api key here - https://api.nasa.gov/
import Config from './config.js';

const API = 'https://api.nasa.gov/mars-photos/api/v1/rovers';
const api_key = Config.api_key;

export default {

  name: 'App',

  data () {
    return {
      page:1,
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
            })
            .catch((error) => {
              console.log(error);
            });
    }
  },

  components: {
    Picker,
    HeroImage,
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
</style>