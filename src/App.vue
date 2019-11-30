<template>
  
<div class="hello"> 

<header>
  <h2> {{msg}}</h2>
</header>

<section>
	<article>
		
    <p><b>A natural disaster is a major adverse event resulting from natural processes of the Earth; examples are floods, hurricanes, tornadoes, volcanic eruptions, earthquakes, tsunamis, and other geologic processes. A natural disaster can cause loss of life or damage property, and typically leaves some economic damage in its wake, the severity of which depends on the affected population's resilience (ability to recover) and also on the infrastructure available.</b></p>
  </article>

<article id="buttons">
<button v-on:click="earthquakes">Significant earthquakes in the past month</button>
</article>


</section>

    <div id="app" ></div>
  </div>
</template>

<script>
export default {
  name: 'hello',
  data () {
    return {
      msg: 'Natural disasters',
      
    }},
  mounted: function() {
            console.log("map: ", google.maps)
            this.map = new google.maps.Map(document.getElementById('app'), {
            center: {lat: 6.6131, lng: 0.1864},
            scrollwheel: false,
            zoom: 2.7,
            mapTypeId: 'terrain'
            }) 
            
        
        
     
},

methods: {
    earthquakes: function (event) {
      if (event) {

let script = document.createElement('script');
        script.setAttribute('src','https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/significant_month.geojsonp')
        document.getElementsByTagName('head')[0].appendChild(script)

         

          window.eqfeed_callback = (result) => {
           return (() => {
          for (var i = 0; i < result.features.length; i++) {
          var coords = result.features[i].geometry.coordinates;
          var latLng = new google.maps.LatLng(coords[1],coords[0]);
          const marker = new google.maps.Marker({
            position: latLng,
            map: this.map
          }); 
          }
})()
}


}
      }
    }



  }



    


</script>

<style >
    #app {
    height:600px;
    width: 100%;
   }

* {
  box-sizing: border-box;
}

body {
  font-family:Tahoma, Geneva, sans-serif ;
  font-size:16px;
}

header {
  
  background-color: #006400 ;
  padding: 30px;
  text-align: center;
  font-size: 35px;
  color: white;
  font-family: Constantia, "Lucida Bright", "DejaVu Serif", Georgia, "serif";
}


article {
  
  padding: 20px;
  width: 100%;
  background-color: #90EE90;
  height: 180px; 
}


article#buttons {
  
  padding: 20px;
  width: 100%;
  background-color: #006400;
  height: 70px; 
}

button {
height: 30px; 
}


section:after {
  content: "";
  display: table;
  clear: both;
}



</style>
