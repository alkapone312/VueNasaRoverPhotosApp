<template>
	<div class="heroImage"></div>
</template>

<script>
export default {

  name: 'HeroImage',

  data () {
    return {

    }
  }
}
</script>

<style lang="scss" scoped>
	.heroImage
	{
		width: 100vw;
		height: 100vh;
		background-image: url("../assets/heroImage.jpeg");
		background-repeat: no-repeat;
		background-size: cover;
		position: absolute;
		left: 0;top: 0;
		z-index: -1;
	}
</style>