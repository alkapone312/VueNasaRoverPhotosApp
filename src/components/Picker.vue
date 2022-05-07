<template>
	<Transition name="view">
	<div class="pickerWrapper" v-if="this.ready">

		<p class="quote">"Almost before we knew it, we had left the ground."</p>

		<label for="rover">Rover:</label>
		<select name="rover" class="pick" v-model="rover">
			<option value="curiosity">Curiosity</option>
			<option value="opportunity">Opportunity</option>
			<option value="spirit">Spirit</option>
		</select>

		<label for="camera">Camera:</label>
		<select name="camera" class="pick" v-model="camera">
			<option value="">(Optional) Pick rover camera</option>
			<option value="FHAZ">Front Hazard Avoidance Camera</option>
			<option value="RHAZ">Rear Hazard Avoidance Camera</option>
			<option value="MAST">Mast Camera</option>
			<option value="CHEMCAM">Chemistry and Camera Complex</option>
			<option value="MAHLI">Mars Hand Lens Imager</option>
			<option value="MARDI">Mars Descent Imager</option>
			<option value="NAVCAM">Navigation Camera</option>
			<option value="PANCAM">Panoramic Camera</option>
			<option value="MINITES">Miniature Thermal Emission Spectrometer</option>
		</select>

		<div class="date" v-if="this.dateType">
			<label for="day">Day</label>
			<label for="month">Month</label>
			<label for="year">Year</label>

			<input type="number" class = "day pick" name="day" min="1" max="31" v-model="day">
			<input type="number" class = "month pick" name="month" min="1" max="12" v-model="month">
			<input type="number" class = "year pick" name="year" min="1980" max="2022" v-model="year">
		</div>

		<div class="sol" v-if="!this.dateType">
			<input type="number" class = "sol pick" name="sol" min="0" v-model="sol">
		</div>

		<div class="dateType" @click="this.dateType = !this.dateType;">
			<div :class="{active : !dateType}">Sols</div>
			<div :class="{active : dateType}">Earth date</div>
		</div>

		<button class="search" @click = "handleRequest">
			Search!
		</button>
	</div>
	</Transition>
</template>

<script>
export default {

  name: 'Picker',

  data () {
    return {
    	dateType: false, //true - earth date, false - sol's
    	ready: false,
    	rover: 'curiosity',
    	camera: '',
    	sol: 0,
    	day: 1,
    	month: 1,
    	year: 1980

    }
  },

  methods: {
    handleRequest () {
      this.$emit('request',{
      		rover: this.rover,
      		camera: this.camera,
      		dateType: this.dateType,
      		sol: this.sol,
      		day: this.day,
      		month: this.month,
      		year: this.year
      	});
    }
  },

  mounted() {
  	this.ready=true;
  }
}
</script>

<style lang="scss" scoped>
	$textOnWhite: #444;

	.pickerWrapper
	{
		display: grid;
		grid-template-rows:2fr 0.5fr 1.5fr 0.5fr 1.5fr 0.5fr 1.5fr 0.2fr 0.3fr;
		width: 80%;
		height: 600px;

		background-color: rgba(255,255,255,0.7);
		box-shadow: 0px 10px 20px -5px #999;
		position: absolute;
		left: 50%;
		top: 50%;
		transform: translate(-50%, -50%);
	}

	.pickerWrapper *
	{
		justify-self: center;
	}

	.quote
	{
		font-size: 1.5em;
		font-weight: 600;
		font-style: italic;
		text-align: center;
		width: 80%;
	}

	.pick
	{
		width: 80%;
		height: 50px;
		text-align: center;
		border:none;
		background-color: rgba(#000, .8);
		font-size: 1.5em;
		margin: 10px;
		transition: border-width .1s ease;
		color: #ccc;
		outline: none;
	}

	.pick:focus
	{
		border: 3px solid #02b6ff;
	}

	label
	{
		position: relative;
		left: -40%;
		top: -10px;
		transform:translate(50%, 50%);
		color: $textOnWhite;
	}

	.date
	{
		text-align: left;
		display: grid;
		grid-template-rows:40% 100%;
		grid-template-columns:20% 20% 1fr;
		grid-gap: 10px;
		width: 80%;

		.day, .month, .year, label
		{
			left: -50%;
			width: 100%;
			margin: 0;
		}
	}

	.sol
	{
		width: 80%;
		display: block;
		flex-direction: column;
		input
		{
			width: 100%;
			margin: 0;
		}

		label
		{

		}
	}

	.dateType
	{
		display: flex;
		flex-direction: row;
		text-align: center;
		width: 100%;
		align-items: center;
		color: $textOnWhite;
		cursor: pointer;

		div
		{
			width: 50%;
		}
	}

	.active
	{
		font-size: 1.2em;
		font-weight: 600;
	}

	.search
	{
		position: relative;
		z-index: 1;
		color: #fff;
		font-size: 1.5em;
		width: 200px;
		height: 50px;
		border:none;
		background-color: rgba(#000, .8);
		cursor: pointer;
		transition: background-color .3s ease;

		&:hover
		{
			background-color: rgba(#000, 0.75);
		}
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

	@media (min-width: 810px) {
		.quote
		{
			font-size: 2em;
		}
	}

	@media (min-width: 1024px) {
		.quote
		{
			font-size: 3em;
		}
	}

</style>