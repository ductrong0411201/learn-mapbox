<template>
  <div class="main">
    <div class="flex">
      <!-- Map Display here -->
      <div class="map-holder" id="comparison-container">
        <div id="map" class="map"></div>
        <div id="map-compare" class="map"></div>
      </div>
    </div>
  </div>
</template>

<script>
import mapboxgl from "mapbox-gl";
import "mapbox-gl/dist/mapbox-gl.css";
import MapboxCompare from "mapbox-gl-compare";
import "mapbox-gl-compare/dist/mapbox-gl-compare.css";
import "@mapbox/mapbox-gl-geocoder/dist/mapbox-gl-geocoder.css";
import RulerControl from "mapbox-gl-ruler-control";
import MapboxDraw from "@mapbox/mapbox-gl-draw";
import "@mapbox/mapbox-gl-draw/dist/mapbox-gl-draw.css";
export default {
  props: {
    isCompare: { type: Boolean, default: false },
    isDashboard: { type: Boolean, default: false },
    isMapview: { type: Boolean, default: false },
    aois: { type: Array, default: () => [] },
    selected: { type: String },
  },
  data() {
    return {
      loading: false,
      location: "",
      access_token: process.env.VUE_APP_MAP_ACCESS_TOKEN,
      center: [103.8088623, 1.3459122],
      zoom: 9,
      map: {},
      marker: null,
      markerstyle: {
        draggable: true,
        color: "#D80739",
      },
      mapCompare: {},
      map2: {},
    };
  },
  mounted() {
    this.createMap();
  },
  watch: {
    selected() {
      if (this.selected == "Jaipur") {
        this.center = [75.8031, 26.8948];
        this.zoom = 9;
      } else if (this.selected == "Jodhpur") {
        this.center = [73.017110, 26.1703594];
        this.zoom = 8;
      } else if (this.selected == "Kota") {
        this.center = [76.1082, 25.1094];
        this.zoom = 7
      } else if (this.selected == "Mumbai") {
        this.center = [72.8356, 18.9817];
        this.zoom = 10
      } else if (this.selected == "Nagpur") {
        this.center = [79.0748, 21.005];
        this.zoom = 8.5
      } else if (this.selected == "Pune") {
        this.center = [73.8493, 18.4594];
        this.zoom = 8.5 
      } else {
        this.center = [103.8088623, 1.3459122];

      }
      this.createMap();
    },
    isCompare() {
      this.createMap(this.isCompare);
      if (this.isCompare == false) {
        this.mapCompare.remove();
        this.map.remove();
        this.map = {};
        this.mapCompare = {};
        this.map2.remove();
        this.map2 = {};
        this.createMap(false);
      }
    },
  },
  methods: {
    createMap(isCompare) {
      try {
        mapboxgl.accessToken = this.access_token;
        this.map = new mapboxgl.Map({
          container: "map",
          style: "mapbox://styles/mapbox/streets-v11",
          attributionControl: false,
          center: this.center,
          zoom: this.zoom,
        });

        this.map.addControl(new mapboxgl.ScaleControl(), "bottom-right");
        this.map.addControl(new mapboxgl.NavigationControl(), "bottom-right");

        if (this.isMapview) {
          const ruler = new RulerControl();
          this.map.addControl(ruler, "bottom-right");
          const draw = new MapboxDraw({
            displayControlsDefault: false,
            controls: {
              polygon: true,
            },
            defaultMode: "draw_polygon",
          });
          this.map.addControl(draw, "bottom-right");
        }
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
        if (isCompare && this.isMapview) {
          this.createMapCompare();
        }
        if (this.isDashboard) {
          this.addAoiToMap();
        }
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
      this.mapCompare = new mapboxgl.Map({
        container: "map-compare",
        style: "mapbox://styles/mapbox/streets-v11",
        attributionControl: false,
        center: this.center,
        zoom: this.zoom,
      });
      const container = "#comparison-container";
      this.map2 = new MapboxCompare(this.mapCompare, this.map, container, {});
    },
    removeCompare() {
      this.mapCompare.remove();
      this.mapCompare = {};
    },
    addAoiToMap() {
      let aoi = this.aois.filter((e) => e.name == this.selected);
      const data = aoi[0].geometry;
      this.map.on("load", () => {
        this.map.addSource("aoi", {
          type: "geojson",
          data: data,
        });
        this.map.addLayer({
          id: "aoi",
          type: "line",
          source: "aoi",
          paint: {
            "line-color": "#000",
            "line-width": 2,
          },
        });
      });
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
  position: relative;
  width: 100%;
  height: 100%;
}
.main {
  height: 100%;
}
.map {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 100%;
}
</style>
