<template>
  <div class="gallery">
      <div class="close" @click="$emit('closeGallery')"></div>

      <TransitionGroup name="showImage">
        <img v-for="i in data.photos.length" :class="{show : show[i-1]}" :key="i-1" class="galleryItem" :src="data.photos[i-1].img_src" @load="showElement(i-1)" @click="$emit('openImage', i-1)"/>
      </TransitionGroup>
  </div>
</template>

<script>
export default {

  name: 'Gallery',

  props: {
    data: {
      type: Object, // String, Number, Boolean, Function, Object, Array
      required: true,
    }
  },

  data () {
    return {
      show: new Array(25).fill(false),
    }
  },

  methods:
  {
    showElement(id)
    {
      this.show[id] = true;
    }
  }
}
</script>

<style lang="scss" scoped>
  .gallery
  {
    display: flex;
    justify-content: center;
    flex-direction: row;
    flex-wrap: wrap;
    width: 100%;
    padding:10%;
    padding-top: 10vh;
    height: 100vh;
    background-color: rgba(255,255,255,1);
    overflow-y: scroll;
  }

  .galleryItem
  {
    opacity: 0;
    height: 300px;
    margin: 5px;
    transition:opacity .5s ease;
    cursor: pointer;
    transition: all .5s ease;

    &:hover
    {
      box-shadow: 0 10px 20px -8px black;
    }
  }

  .showImage-enter-active,
  .showImage-leave-active {
    transition: all 1s ease;
    transition-delay: .1s;
  }

  .showImage-enter-from,
  .showImage-leave-to {
    opacity: 0;
    margin-top: -60px;
  }

  .show
  {
    opacity: 1;
  }

  .close {
  position: absolute;
  right: 32px;
  top: 32px;
  width: 32px;
  height: 32px;
  opacity: 0.3;
  cursor: pointer;

    &:hover {
    opacity: 1;
    }
    &:before, &:after {
      position: absolute;
      left: 15px;
      content: ' ';
      height: 33px;
      width: 2px;
      background-color: #333;
    }
    &:before {
      transform: rotate(45deg);
    }
    &:after {
      transform: rotate(-45deg);
    }
}


</style>