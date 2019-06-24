

<template>
  <div id="bar">
	  <div v-if="typeOfDataIsNumber">
	  	<div :style="styleOfContainer">
	  		<div>{{carData}}
	  		</div>
		  	<div :style="styleOfBar">
		  	</div>
		  	<div :style="styleOfLegend">{{names}}
		  	</div>
	  	</div>  	
	  </div>

  </div>

</template>



<style>

</style>


<script>



	export default {
		name: "bar",
		data() {
			return{
				heigthOfChart : 700 ,
			}
		},
		props : ['carData', 'scale', 'names'],
		methods:{
			calulateHeight() {
				return (Math.round(this.carData/this.scale*this.heigthOfChart) +"px")
			},
			calculMarginTop() {
				return (this.scale-this.carData)/this.scale*this.heigthOfChart +"px"
			}
		},
		computed : {
			styleOfBar(){
				return {backgroundColor:"blue",width:"30px",height:this.calulateHeight(), marginLeft:"0px", marginTop:"0px", display: "flex"}
			},
			styleOfContainer(){
				return {marginLeft:"10px", marginTop:this.calculMarginTop()}
			},
			styleOfLegend(){
				return {width:"15px", writingMode: "vertical-rl", textOrientation: "mixed"}
			},
			typeOfDataIsNumber(){
				if(isNaN(parseFloat(this.carData))){	
					return false
				}
				return true
			},
		}
	}

	
</script>