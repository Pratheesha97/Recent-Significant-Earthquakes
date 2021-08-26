<!-- ========================== Template ========================== -->
<template>
  <div class="vuemap">
    <header>
      <h2>{{ headline }}</h2>
    </header>

    <section>
      <article id="buttons">
        <button v-on:click="earthquakes">
          Click here to see Significant Earthquakes in the past month
        </button>
      </article>
    </section>

    <div id="app"></div>
  </div>
</template>

<!-- ========================== Script ========================== -->
<script>
export default {
  name: "vuemap",
  data() {
    return {
      headline: "Natural disasters - Earthquakes",
    };
  },
  mounted: function() {
    console.log("map: ", google.maps);
    this.map = new google.maps.Map(document.getElementById("app"), {
      center: { lat: 6.6131, lng: 0.1864 },
      scrollwheel: false,
      zoom: 2.7,
      mapTypeId: "terrain",
    });
  },

  methods: {
    earthquakes: function(event) {
      if (event) {
        let script = document.createElement("script");
        script.setAttribute(
          "src",
          "https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/significant_month.geojsonp"
        );
        document.getElementsByTagName("head")[0].appendChild(script);

        window.eqfeed_callback = (result) => {
          return (() => {
            for (var i = 0; i < result.features.length; i++) {
              var coords = result.features[i].geometry.coordinates;
              var latLng = new google.maps.LatLng(coords[1], coords[0]);
              const marker = new google.maps.Marker({
                position: latLng,
                map: this.map,
              });
            }
          })();
        };
      }
    },
  },
};
</script>

<!-- ========================== CSS Styles ========================== -->
<style>
#app {
  height: 450px;
  width: 100%;
}

* {
  box-sizing: border-box;
}

body {
  font-family: Tahoma, Geneva, sans-serif;
  font-size: 16px;
}

header {
  background-color: #000000;
  opacity: 0.9;
  padding: 10px;
  text-align: center;
  font-size: 24px;
  color: white;
  font-family: Constantia, "Lucida Bright", "DejaVu Serif", Georgia, "serif";
}

article#buttons {
  padding: 20px;
  width: 100%;
  background-color: #dddeee;
  height: 70px;
  text-align: center;
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
