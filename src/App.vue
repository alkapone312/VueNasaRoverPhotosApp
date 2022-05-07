<template>
  <div class="wrapper">
    <HeroImage />
    <Picker />
  </div>
</template>

<script>
import HeroImage from './components/HeroImage.vue';
import Picker from './components/Picker.vue';

export default {

  name: 'App',

  data () {
    return {

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