<template>
  <b-container>
    <b-row>
      <b-col>
        <h1>Province of British Columbia</h1>
      </b-col>
    </b-row>


    <b-row>

      <b-col id="selectLocationContainer" cols="3">
        <label for="selectLocation">Select aquifer location:</label>
        <b-form-select id="selectLocation" v-model="selectedLocation" :options="aquiferLocations"></b-form-select>
      </b-col>

      <b-col id="selectedLocationContainer">
        Selected location:<br/>
        <b-alert show><strong>{{ selectedLocation }}</strong></b-alert>
	<span v-html="details"></span>
      </b-col>


    </b-row>
  </b-container>
</template>

<script>
import AQUIFERS from '@/data/aquifers.json'
const SELECTPLACEHOLDER = 'Please select a location'

export default {
  components: {
  },
  data() {
	let locations=(AQUIFERS.results)
		.filter(item=>item.location)
.filter(item=>item.location!='')
		.map(function(item){
			let x=item.location.trim();
			return {value:x,text:x}
		})
		.sort(function(a, b) {
var uppera=a.value.toUpperCase();
var upperb=b.value.toUpperCase();
if (uppera < upperb) { return -1 }
if (uppera > upperb) { return 1 }
return 0
})		
.filter(function(el, idx, arr) {
return !idx || el.value != arr[idx - 1].value
//        		return arr.indexOf(el)==idx;
})
		;

          // Insert SelectPlaceHolder prompt
        locations.unshift({value: SELECTPLACEHOLDER, text: SELECTPLACEHOLDER, disabled: true })

	let locationsData=(AQUIFERS.results)
		.filter(item=>item.location)
.filter(item=>item.location!='')
		.map(function(item){
			item.location=item.location.trim();
			return item;
		})
		;
    return {
      selectedLocation: SELECTPLACEHOLDER,
	aquifers: locationsData, // TODO: import aquifers from an API
	aquiferLocations:locations,
	details:'',
	properties:["aquifer_id",
		"mapping_year",
//		"id",
		"name",
		"area",
//		"lsu",
//		"location",
//		"demand",
//		"material",
//		"subtype",
		"vulnerability"]
//		"productivity"]
    }
  },
  computed: {
  },
  watch:{
	selectedLocation:function(val) {
	let selectedLocationData=this.aquifers.filter(
		item => item.location.trim()==val)
	this.details='';

	var detailsstr;
	detailsstr='';

		detailsstr=detailsstr+'<table border=1>';
		detailsstr=detailsstr+'<tr><th>Aquifer Id</th><th>Mapping Year</th><th>Name</th><th>Area</th><th>Vulnerability</th></tr>';

/*		detailsstr=detailsstr+'<tr>';
	for (var i=0;i<this.properties.length;i++){
		detailsstr=detailsstr+'<th>'+this.properties[i]+'</th>';
	}
		detailsstr=detailsstr+'</tr>';
*/
	for (var i=0;i<selectedLocationData.length;i++) {
		detailsstr=detailsstr+'<tr>';
/*		detailsstr=detailsstr+'<td>'+selectedLocationData[i]["auifqe_id"]+'</td>'+'<td>'+selectedLocationData[i]["mapping_year"]+'</td>'+'<td>'+selectedLocationData[i]["name"]+'</td>'+'<td>'+selectedLocationData[i]["area"]+'</td>'+'<td>'+selectedLocationData[i]["vulnerability"]+'</td>';
*/

		for (var j=0;j<this.properties.length;j++){
//			detailsstr=detailsstr+'<td>'+((selectedLocationData[i][this.properties[j]]!=null) ?
//				(JSON.stringify(selectedLocationData[i][this.properties[j]])).replace(/\"/g,"") : '')+'</td>';
			detailsstr=detailsstr+'<td>'+((selectedLocationData[i][this.properties[j]]!=null) ?
				(JSON.stringify(selectedLocationData[i][this.properties[j]])).replace(/"/g,"") : '')+'</td>';
		}
		detailsstr=detailsstr+'</tr>';
	}
		detailsstr=detailsstr+'</table>';







	this.details=detailsstr;
	}
}



};
</script>

<style scoped>
</style>