<template>
  <l-map style="height: 100%" :zoom="zoom" :center="center">
    <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
    <v-geosearch :options="geosearchOptions" ></v-geosearch>
    <l-marker :lat-lng="markerLatLng" :icon="mdi" @click="show = !show">
    </l-marker>
    <l-polygon :lat-lngs="polygon.latlngs" :color="polygon.color"></l-polygon>
    <v-tooltip
    v-model="show"
    color="success"
    >
    Bandung Digital Valley
    </v-tooltip>
  </l-map>
</template>

<script>
import { LMap, LTileLayer, LMarker, LPolygon } from "vue2-leaflet";

import Vue from "vue";
import axios from "axios";
import VueAxios from "vue-axios";

import VGeosearch from 'vue2-leaflet-geosearch'
import {OpenStreetMapProvider} from 'leaflet-geosearch'

Vue.use(VueAxios, axios);

export default {
  name: "Home",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPolygon,
    VGeosearch
  },

  data() {
    return {
      show: false,
      geosearchOptions: { // Important part Here
        provider: new OpenStreetMapProvider(),
      },
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      zoom: 20,
      center: [-6.873153225866675, 107.58658763001276],
      markerLatLng: [-6.873153225866675, 107.58658763001276],
      polygon: {
        latlngs: [],
        color: "red",
      },
      list: [],
      listPoly: [],
      // address: []
    };
  },
  mounted() {
    Vue.axios
      .get("https://4hhqult8.directus.app/items/tbl_maps_detail")
      .then((resp) => {
        this.list = resp.data.data;
        this.polygon.latlngs = this.mapPoly(this.list);
        console.warn(resp.data.data);
      });

      Vue.axios
  },
  methods: {
    mapPoly(array) {
      let data = [];
      let maps_id = {};
      array.forEach((items) => {
        if (maps_id[items.maps_id]) {
          maps_id[items.maps_id].push([
            Number(items.latitude),
            Number(items.longitude),
          ]);
        } else {
          maps_id[items.maps_id] = [
            [Number(items.latitude), Number(items.longitude)],
          ];
        }
      });
      console.log(maps_id, 'iyooow')
      for (const key in maps_id) {
        data.push([maps_id[key]]);
      }
      console.log(data, "wakwaaaawwww");
      return data;
    },
  },
};
</script>
