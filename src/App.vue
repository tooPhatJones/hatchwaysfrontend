<template>
  <div id="app">
    <div class="header">
      <label for="filter">Filter by worker name</label>
      <br />
      <input type="text" name="filter" v-model="searchname" v-on:input="filter" />
      <br />
      <label for="filter">Toggle sort by date</label>
      <input type="checkbox" name="filter" v-on:click="sortdate" />
      <br />
      Number of work orders showing: {{numofworkorders}}
    </div>

    <div v-for="order in workorders" v-bind:key="order.id">
      <workorders :order="order"></workorders>
    </div>
  </div>
</template>

<script>
import workorders from "./components/workorders.vue";
import axios from "axios";
var moment = require("moment");
export default {
  name: "app",
  data: function() {
    return {
      workorders: {}, //hold the data and mutatable
      Savedworkorders: {}, //save a copy of the origional data
      searchname: "", //used to get the serach value
      numofworkorders: 0, //used for th workorder display counter
      sortvalue: true //used to keep track of the sort order.
    };
  },
  methods: {
    filter: function() {
      //workorders set to saved value so we dont loose track
      this.workorders = this.Savedworkorders;
      var searchname = this.searchname.toLowerCase();

      let result = [];
      result = this.workorders.filter(oneorder => {
        return oneorder.worker.name.toLowerCase().search(searchname) != -1;
      });

      this.workorders = result;
      this.numofworkorders = result.length;
    },
    sortdate: function() {
      //if true, sort asc, if false, sort desc
      if (this.sortvalue) {
        this.workorders.sort(function(a, b) {
          return new Date(a.deadline) - new Date(b.deadline);
        });
      } else {
        this.workorders.sort(function(a, b) {
          return new Date(b.deadline) - new Date(a.deadline);
        });
      }
      this.sortvalue = !this.sortvalue;
    }
  },
  components: {
    workorders
  },
  async mounted() {
    //get the list of workorders
    await axios({
      method: "GET",
      url: "https://www.hatchways.io/api/assessment/work_orders"
    }).then(res => {
      this.workorders = res.data.orders;
    });

    //loop through workorders and set the apropriate worker to the workorder.
    for (var i = 0; i < this.workorders.length; i++) {
      await axios({
        method: "GET",
        url:
          "https://www.hatchways.io/api/assessment/workers/" +
          this.workorders[i].workerId
      }).then(res => {
        var tempdate = new Date(this.workorders[i].deadline);
        this.workorders[i].deadline = moment(tempdate).format("LLL");
        this.workorders[i].worker = res.data.worker;
      });
    }
    this.Savedworkorders = this.workorders;
    this.numofworkorders = this.workorders.length;
    this.sortdate();
    console.log(this.workorders)
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.header {
  margin: 50px;
}
</style>
