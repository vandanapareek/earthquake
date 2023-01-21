<template>
<div class="content">
     <div class="flex-container">
          <div class="flex-child left-div">
              <table id="tableComponent" class="table table-bordered table-striped">
                  <thead>
                    <tr>
                      <!-- loop through each value of the fields to get the table header -->
                      <th  v-for="field in theads" :key='field' @click="sortTable(field)" > 
                        {{field}} <i class="bi bi-sort-alpha-down" aria-label='Sort Icon'></i>
                      </th>
                    </tr>
                  </thead>
                  <tbody>
                      <!-- Loop through the list to get the each earthquake detailed data -->
                      <tr v-for="item in items" :key='item'>
                      <td >{{item.title}}</td>
                      <td>{{item.mag}}</td>
                      <td>{{item.url}}</td>
                      <td>{{item.place}}</td>
                    </tr>
                  </tbody>
              </table> 
          </div>

          <div class="flex-child right-div">
                <GoogleMap
                  api-key="AIzaSyBc0kVzrUeEzW81aW02aZHGbYpu0u5wsds"
                  style="width: 100%; height: 650px"
                  :center="center"
                  :zoom="2"
                >
                  <!-- Loop through the list to get the each earthquake marker -->
                  <Marker v-for="(location, i) in locations" :options="{ position: {lat:location.geometry.coordinates[1],lng: location.geometry.coordinates[0]}}" :key="i">
                      
                      <InfoWindow>
                        <div id="contet">
                          <div id="siteNotice"></div>
                          <h1 id="firstHeading" class="firstHeading">{{ location.properties.place }}</h1>
                          <div id="bodyContent">
                            <p>
                              Date & Time: {{ getDate(location.properties.time) }}
                            </p>
                          </div>
                        </div>
                      </InfoWindow>
                  </Marker>
                </GoogleMap>
          </div>
      </div>
    </div>
</template>

<script>

/**
 * @author Vandana Pareek
 * @version 0.0.1
 */

import { defineComponent } from "vue";
import { GoogleMap, Marker, InfoWindow } from "vue3-google-map";
import axios from 'axios'

export default defineComponent({
  components: { GoogleMap, Marker, InfoWindow },
  data() {
            return {
              center: { lat: -28.024, lng: 140.887 },
              locations:[],
              items:[],
              theads:["Title","MAG","URL", "Place"]
            }
        },
  async mounted() {

      //Retrieve earthquake data
      axios.get("https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_day.geojson").then(response => {
            this.locations = response.data.features;

            this.items = this.locations.filter((e) => e.properties.title != "").map((e) => { return {title: e.properties.title, place: e.properties.place, url:e.properties.url, mag:e.properties.mag}});

            this.items = this.items.sort((a, b) => b.mag - a.mag); 
        }).catch((error) => {
        console.log(error);
        })
    },
    methods:{
      getDate(UNIX_timestamp) {
        //Retrieve date & time from timestamp
        var date = new Date(UNIX_timestamp).toLocaleDateString("SGT")
        var time = new Date(UNIX_timestamp).toLocaleTimeString("SGT")
        return date + " " + time 
        }
    }
});
</script>