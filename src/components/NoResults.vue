<template>
	<div class="noResults">There is no results with query like this</div>
</template>

<script>
export default {

  name: 'NoResults',

  data () {
    return {

    }
  }
}
</script>

<style lang="css" scoped>
	.noResults
	{
		display: flex;
		position: absolute;
		left: 50%;
		top: 10px;
		z-index: 999;
		width: 30%;
		height: 40px;
		background-color: rgba(255,255,255,.8);
		color: #444;
		border-radius: 5px;
		align-items: center;
		justify-content: center;
		transform: translate(-50%);
	}
</style>