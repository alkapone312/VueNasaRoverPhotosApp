<template>
  <div class="wrapper">

    <HeroImage />

    <Transition name="pickerShow">
      <Picker v-if="this.ready && this.mode === 0" @request="handleRequest"/>
    </Transition>

    <Transition name="fade">
      <Gallery v-if="mode=== 1" :data="data" @openImage="openImage" @closeGallery="this.mode=0; this.data=null"/>
    </Transition>

    <Transition name="noResults">
      <NoResults v-if="this.noResults"/>
    </Transition>

    <Transition name=fade>
      <Viewer v-if="showImage" :image="image" @closeImage="this.showImage = false"/>
    </Transition>

  </div>
</template>

<script>
import axios from 'axios';

import HeroImage from './components/HeroImage.vue';
import Picker from './components/Picker.vue';
import NoResults from './components/NoResults.vue';
import Gallery from './components/Gallery.vue';
import Viewer from './components/Viewer.vue';
//create your own api key here - https://api.nasa.gov/
import Config from './config.js';

const API = 'https://api.nasa.gov/mars-photos/api/v1/rovers';
const api_key = Config.api_key;

export default {

  name: 'App',

  components: {
    Viewer,
    Gallery,
    NoResults,
    Picker,
    HeroImage,
  },

  data () {
    return {
      noResults: false,
      ready: false,
      page:1,
      mode: 0, // 0 - search, 1 - browse picks
      data: null,
      showImage: false,
      image:null
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
    },

    openImage(index)
    {
      this.showImage = true;
      this.image=this.data.photos[index];
    }
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

  .pickerShow-enter-active,
  .pickerShow-leave-active {
    transition: all 1s ease;
    transition-delay: .1s;
  }

  .pickerShow-enter-from,
  .pickerShow-leave-to {
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

  .fade-enter-active,
  .fade-leave-active {
    transition: all 1s ease;
  }

  .fade-enter-from,
  .fade-leave-to {
    opacity: 0;
  }
</style>