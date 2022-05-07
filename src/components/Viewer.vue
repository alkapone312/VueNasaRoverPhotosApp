<template>
	<div class="viewerWrapper">
		<div class="viewer" :style="style">
	    	<div v-show="show" class="close" @click="$emit('closeImage')"></div>
		</div>
	</div>
</template>

<script>
export default {

  name: 'Viewer',

  props: {
    image: {
      type: String, // String, Number, Boolean, Function, Object, Array
      required: true,
    }
  },

  data () {
    return {
    	show:false,
    }
  },

  computed: {
    style () {
      return `background-image:url("${this.image}");`;
    }
  },

  mounted()
  {
  	setTimeout(()=>{this.show = true}, 100)
  }
}
</script>

<style lang="scss" scoped>

	.viewerWrapper
	{
		position: fixed;
		left: 0;
		top: 0;
		width: 100vw;
		height: 100vh;
		background-color:rgba(0,0,0,0.7);
	}

	.viewer
	{
		position: absolute;
		left: 50%;
		top: 50%;
		transform: translate(-50%, -50%);
		width:50vw;
		height: 600px;
		background-color: #000;
		background-size:contain;
		background-repeat:no-repeat;
		background-position: center;
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
			background-color: #BBB;
		}
		&:before {
			transform: rotate(45deg);
		}
		&:after {
			transform: rotate(-45deg);
		}
}

@media (max-width: 810px) {
	.viewer
	{
		width: 90vw;
	}
}

@media (max-width: 575px) {
	.viewer
	{
		width: 90vw;
		height: 400px;
	}
}
</style>