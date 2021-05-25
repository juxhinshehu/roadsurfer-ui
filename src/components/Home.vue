<template>
  <div class="Home">
    <form class="form-inline mb-5">
      <div class="form-group">
        <label class="mr-1"><b>Station</b></label>
        <select class="mr-2" @change="stationChanged($event)">
          <option v-for="station in stations" 
                  v-bind:key="station.name"
                  :value="station.id">
            {{ station.name }}
          </option>
        </select>
      </div>
      <div class="form-group">
        <label class="mr-1"><b>Day</b></label>
        <div class="mr-2">
          <datepicker @selected="dayChanged" :disabled-dates="disabledDates"></datepicker>
        </div>
      </div>
      <button class="btn btn-info btn-sm" v-on:click="getAvailabilities">Check</button>
    </form>
    <div class="row">
    <table class="table" v-if="availabilities.length > 0">
        <thead>
        <tr>
            <th scope="col">Name</th>
            <th scope="col">Nr Booked</th>
            <th scope="col">Nr Available</th>
        </tr>
        </thead>
        <tbody>
          <tr v-for="availability in availabilities" v-bind:key="availability.name">
              <td>{{ availability.name }}</td>
              <td>{{ availability.bookingsCounter }}</td>
              <td>{{ availability.available }}</td>
          </tr>
        </tbody>
    </table>
  </div>
  </div>
</template>

<script>
import Datepicker from 'vuejs-datepicker';
const axios = require('axios');

export default {
  name: 'Home',
  props: {
  },
  data: function() {
    return {
      availabilities:[],
      stations: [],
      selectedDay: null,
      selectedStation: null,
      disabledDates: {
        to: new Date(),
      }
    }
  },
  components: {
    Datepicker
  },
  mounted: function() {
    this.getStations()
  },
  methods: {
    dayChanged(date) {
      this.selectedDay = date.toISOString().slice(0, 10);
    },
    getAvailabilities(event) {
      event.preventDefault();
      if (this.selectedStation == null || this.selectedDay ==  null) {
        alert("Please select a day and a camp");
      } else {
        axios
          .get(`${process.env.VUE_APP_API_URL}/availabilities/${this.selectedStation}/${this.selectedDay}`)
          .then(response => (this.availabilities = response.data ))    
      }
          
    },
    getStations() {
      axios
      .get(`${process.env.VUE_APP_API_URL}/stations`)
      .then(response => {
        this.stations = response.data;
        this.selectedStation = this.stations[0].id;
      })      
    },
    stationChanged(event) {
      console.log(event.target.value)
    }
  }
}
</script>