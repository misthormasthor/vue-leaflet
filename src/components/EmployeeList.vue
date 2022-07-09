<template>
  <div>
    <v-simple-table>
      <thead>
        <tr>
          <th>Latitude</th>
          <th>Longitude</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in list" :key="item.id">
          <td>{{ item.latitude }}</td>
          <td>{{ item.longitude }}</td>
        </tr>
      </tbody>
    </v-simple-table>
  </div>
</template>

<script>
import Vue from "vue";
import axios from "axios";
import VueAxios from "vue-axios";

Vue.use(VueAxios, axios);
export default {
  name: "EmployeeList",
  data() {
    return {
      list: [],
    };
  },
  mounted() {
    Vue.axios
      .get("https://4hhqult8.directus.app/items/tbl_maps_detail")
      .then((resp) => {
        this.list = resp.data.data;
        console.warn(resp.data.data);
      });
  },
};
</script>
