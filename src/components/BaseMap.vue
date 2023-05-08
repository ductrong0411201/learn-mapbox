<template>
  <div class="main">
    <div class="flex">
      <!-- Map Display here -->
      <div class="map-holder" id="comparison-container">
        <div id="map"></div>
        <div id="map-compare"></div>
      </div>
    </div>
  </div>
</template>

<script>
import mapboxgl from "mapbox-gl";
import * as mapboxCompare from 'mapbox-gl-compare';
import "@mapbox/mapbox-gl-geocoder/dist/mapbox-gl-geocoder.css";
import RulerControl from "mapbox-gl-ruler-control";
export default {
  props: {
    isCompare: Boolean,
  },
  data() {
    return {
      loading: false,
      location: "",
      access_token: process.env.VUE_APP_MAP_ACCESS_TOKEN,
      center: [103.8088623, 1.3459122],
      map: {},
      marker: null,
      markerstyle: {
        draggable: true,
        color: "#D80739",
      },
      mapCompare: {},
    };
  },
  mounted() {
    this.createMap();
  },
  computed: {
    compareMod() {
      if (this.isCompare) {
        this.createMapCompare();
      }
      return this.isCompare;
    },
  },
  methods: {
    createMap() {
      try {
        mapboxgl.accessToken = this.access_token;
        this.map = new mapboxgl.Map({
          container: "map",
          style: "mapbox://styles/mapbox/streets-v11",
          center: this.center,
          zoom: 10.5,
        });
        this.map.addControl(new mapboxgl.ScaleControl(), "bottom-right");
        this.map.addControl(new mapboxgl.NavigationControl(), "bottom-right");
        const ruler = new RulerControl();
        this.map.addControl(ruler, "bottom-right");
        this.map.addControl(
          new mapboxgl.GeolocateControl({
            positionOptions: {
              enableHighAccuracy: true,
            },
            trackUserLocation: true,
            showUserHeading: true,
          }),
          "bottom-right"
        );
        this.map.on("click", this.handleMapClick);
      } catch (err) {
        console.log("map error", err);
      }
    },
    handleMapClick(event) {
      if (this.marker) {
        this.marker.remove();
      }

      const { lngLat } = event;
      this.marker = new mapboxgl.Marker(this.markerstyle)
        .setLngLat(lngLat)
        .addTo(this.map);
    },
    createMapCompare() {
      mapboxgl.accessToken = this.access_token;
      const mapCompare = new mapboxgl.Map({
        container: "map-compare",
        style: "mapbox://styles/mapbox/streets-v11",
        center: this.center,
        zoom: 10.5,
      });
      const container = "#comparison-container";
      this.mapCompare = new mapboxCompare(this.map, mapCompare, container, {
          mousemove: true,
          slider: true,
      });
      // mapboxgl.accessToken = this.access_token;
      // const beforeMap = new mapboxgl.Map({
      //   container: "map",
      //   // Choose from Mapbox's core styles, or make your own style with Mapbox Studio
      //   style: "mapbox://styles/mapbox/streets-v11",
      //   center: [0, 0],
      //   zoom: 0,
      // });

      // const afterMap = new mapboxgl.Map({
      //   container: "map-compare",
      //   style: "mapbox://styles/mapbox/streets-v11",
      //   center: [0, 0],
      //   zoom: 0,
      // });

      // // A selector or reference to HTML element
      // const container = "#comparison-container";

      // this.map = new mapboxgl.Compare(beforeMap, afterMap, container, {
      //   // Set this to enable comparing two maps by mouse movement:
      //   mousemove: true,
      //   slider: true,
      // });
    },
  },
};
</script>

<style scoped>
.flex {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  width: 100%;
  height: 100%;
}

.map-holder {
  width: 100%;
  height: 100%;
}
.main {
  height: 100%;
}
#map {
  /* min-height: calc(40vh - 128px); */
  height: 100%;
  width: 100%;
}
</style>
