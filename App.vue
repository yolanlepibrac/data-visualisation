



<template>
  <div id="app" :style="styleApp">
  	<div id="menu" :style="styleMenu">

  		<input type="file" @change="onFileChange" accept=".json" :style="styleInput">

  		<div :style="styleOrder">
  			<input type="radio" id="normal" value="normal" v-model="order" v-on:change="sortedData">
			<label for="normal">normal</label>
			<br>
			<input type="radio" id="acending" value="acending" v-model="order" v-on:change="sortedData">
			<label for="acending">acending</label>
			<br>
			<input type="radio" id="descending" value="descending" v-model="order" v-on:change="sortedData">
			<label for="descending">descending</label>
			<br>
  		</div>


  		<div v-for="(value, property) in fileinput.data[0]" :style="styleProperty">
  			<input type="radio" id="property" :value="property" v-model="propertyPicked" v-on:change="sortedData">
  			<label for="value">{{property}}</label>
			<br>
  		</div>

  		
  	</div>

  	<li v-if="typeOfDataIsNumber">
  		<div v-for="eachcar in dataToDisplay" :style="styleChart">
	  		<bar :carData="eachcar[propertyPicked]" :scale="calulateScale" :names="Object.values(eachcar)[0]">
	  		</bar>
	  	</div>
  	</li>
  	<div v-else></br></br>Impossible to create a graph with this property, please choose quantifiable property
	</div>


  </div>

</template>



<style>

</style>


<script>

	import * as d3 from "d3";
	import JsonData from "./cars.json";
	import bar from "./bar";


	console.log(JsonData);
	
	var reader = new FileReader(); 

	export default {
		data() {
			return {
				fileinput : JsonData,
				dataToDisplay : JsonData.data,
				file: '',
				propertyPicked: "none",
				order : "normal"
			}
		},
		components: {
		    bar,
		 },
		 methods:{
		 	onFileChange(e) {
	            var files = e.target.files || e.dataTransfer.files;
	            if (!files.length)
	                return;
	            this.createInput(files[0]);
	        },
	        createInput(file) {
	            var reader = new FileReader();
	            var vm = this;
	            reader.onload = (e) => {
	            	vm.fileinputresult = reader.result;
	            	this.load(this.fileinputresult);
	            }
	            reader.readAsText(file);    
	        },
	        load(objectChoosen){
	        	var newObject = (JSON.parse(objectChoosen))
	        	console.log((newObject));
	        	this.fileinput = newObject;
	        },
			sortedData(){
				if(this.order === "acending"){
					this.dataToDisplay = Object.create(this.fileinput.data).sort(this.acending(this.propertyPicked))
				}else if(this.order === "descending"){
					this.dataToDisplay = Object.create(this.fileinput.data).sort(this.descending(this.propertyPicked))
				}else if(this.order === "normal"){
					this.dataToDisplay = this.fileinput.data
				}
				console.log(this.dataToDisplay)
			},
			acending( property ) {
				return function( a, b ) {
				  if ( a[property] < b[property]){
				    return -1;
				  }
				  if ( a[property] > b[property] ){
				    return 1;
				  }
				  return 0;
				};
			},
			descending( property ) {
				return function( a, b ) {
				  if ( a[property] > b[property]){
				    return -1;
				  }
				  if ( a[property] < b[property] ){
				    return 1;
				  }
				  return 0;
				};
			},
			
		},
		computed : {
			styleChart(){
				return {display: "inline-block", width: "auto"}
			},
			styleApp(){
				return {display: "inline"}
			},
			styleMenu(){
				return {display: "flex"}
			},
			styleOrder(){
				return {minWidth: "150px"}
			},
			styleInput(){
				return {backgroundColor: "#EEEEEE"}
			},
			styleProperty(){
				return {width: "auto", display: "inline-block", margin:"10px"}
			},
			calulateScale() {
				let valueMaximum = 0
				let lengthTable = this.fileinput.data.length
				for (var i=0;i<lengthTable;i++){
					if(this.fileinput.data[i][this.propertyPicked] > valueMaximum ){
						valueMaximum = this.fileinput.data[i][this.propertyPicked]
					}
				}
				console.log(valueMaximum)
				return valueMaximum
			},
			typeOfDataIsNumber(){
				let nameOfProperty = (this.fileinput.data[0][this.propertyPicked])
				console.log((nameOfProperty))
				if(nameOfProperty && nameOfProperty.toLowerCase() === "double" || nameOfProperty && nameOfProperty.toLowerCase() === "int" ){
					console.log(nameOfProperty.toUpperCase())	
					return true
				}			
				return false
			},
		}
	}

	
</script>