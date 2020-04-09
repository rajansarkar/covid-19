<template>
  <div class="home-content">
    <div class="container py-5">
      <div class="row">
        <div class="col-sm-12">
          <div class="card text-center border-0">
            <div class="card-header border-0">
              <h2>COVID-19 Coronavirus OutBreak</h2>
              <small v-if="isLoad">Last Updated: {{totalData.updated | dateFormate}}</small>
              <loading :colorName="'text-dark'" v-else></loading>
            </div>
          </div>
        </div>
      </div>
      <div class="row">
        <div class="col-sm-3">
          <div class="card border-light my-3 text-center">
            <div class="card-body">
              <h5 class="card-title">Affected Country:</h5>
              <p class="card-text h1 text-secondary" v-if="isLoad">
                <strong>{{countryDatas.length | countFormat}}</strong>
              </p>
              <loading :colorName="'text-dark'" v-else></loading>
            </div>
          </div>
        </div>
        <div class="col-sm-3">
          <div class="card border-light my-3 text-center">
            <div class="card-body">
              <h5 class="card-title">Coronavirus Cases:</h5>
              <p class="card-text h1" v-if="isLoad">
                <strong>{{totalData.cases | countFormat}}</strong>
              </p>
              <loading :colorName="'text-dark'" v-else></loading>
            </div>
          </div>
        </div>
        <div class="col-sm-3">
          <div class="card border-light my-3 text-center">
            <div class="card-body">
              <h5 class="card-title">Deaths:</h5>
              <p class="card-text h1 text-danger" v-if="isLoad">
                <strong>{{totalData.deaths | countFormat}}</strong>
              </p>
              <loading :colorName="'text-danger'" v-else></loading>
            </div>
          </div>
        </div>
        <div class="col-sm-3">
          <div class="card border-light my-3 text-center">
            <div class="card-body">
              <h5 class="card-title">Recovered:</h5>
              <p class="card-text h1 text-success" v-if="isLoad">
                <strong>{{totalData.recovered | countFormat}}</strong>
              </p>
              <loading :colorName="'text-success'" v-else></loading>
            </div>
          </div>
        </div>
      </div>
      <div class="content-bootm mt-5">
        <div class="row">
          <div class="col-sm-2">
            <div class="card border-light my-3 text-center">
              <div class="card-header border-0">Bangladesh</div>
              <div class="card-body p-0">
                <ul v-if="isLoadBd" class="list-group">
                  <li
                    class="px-2 list-group-item d-flex justify-content-between align-items-center"
                  >
                    Cases
                    <span
                      class="badge badge-secondary badge-pill"
                    >{{bdtData.cases | countFormat}}</span>
                  </li>
                  <li
                    class="px-2 list-group-item d-flex justify-content-between align-items-center"
                  >
                    Today Cases
                    <span
                      class="badge badge-dark badge-pill"
                    >{{bdtData.todayCases | countFormat}}</span>
                  </li>
                  <li
                    class="px-2 list-group-item d-flex justify-content-between align-items-center"
                  >
                    Deaths
                    <span
                      class="badge badge-danger badge-pill"
                    >{{bdtData.deaths | countFormat}}</span>
                  </li>
                  <li
                    class="px-2 list-group-item d-flex justify-content-between align-items-center"
                  >
                    Today Deaths
                    <span
                      class="badge badge-danger badge-pill"
                    >{{bdtData.todayDeaths | countFormat}}</span>
                  </li>
                  <li
                    class="px-2 list-group-item d-flex justify-content-between align-items-center"
                  >
                    Recovered
                    <span
                      class="badge badge-success badge-pill"
                    >{{bdtData.recovered | countFormat}}</span>
                  </li>
                  <li
                    class="px-2 list-group-item d-flex justify-content-between align-items-center"
                  >
                    Active
                    <span class="badge badge-info badge-pill">{{bdtData.active | countFormat}}</span>
                  </li>
                  <li
                    class="px-2 list-group-item d-flex justify-content-between align-items-center"
                  >
                    Critical
                    <span
                      class="badge badge-warning badge-pill"
                    >{{bdtData.critical | countFormat}}</span>
                  </li>
                </ul>
                <loading :colorName="'text-dark'" v-else></loading>
              </div>
            </div>
          </div>
          <div class="col-sm-10">
            <div class="row">
              <div class="col-sm-8">
                <h3 v-if="Object.entries(filterData).length <= 0">Top 10 Coronavirus Cases Country</h3>
                <h3 v-else>{{filterData.country}}</h3>
              </div>
              <div class="col-sm-4">
                <select
                  v-model="countryName"
                  @click.prevent="getDataByCountry()"
                  class="form-control form-control-sm"
                >
                  <option value>--Select Country--</option>
                  <option
                    v-for="(country, i) in countryDatas"
                    :value="country.country"
                    :key="i"
                  >{{country.country}}</option>
                </select>
              </div>
            </div>
            {{filterData.length}}
            <table class="table table-bordered text-center mt-3">
              <thead class="thead-light">
                <tr>
                  <th scope="col">Country</th>
                  <th scope="col">Cases</th>
                  <th scope="col">Today Cases</th>
                  <th scope="col">Deaths</th>
                  <th scope="col">Today Deaths</th>
                  <th scope="col">Recovered</th>
                  <th scope="col">Active</th>
                  <th scope="col">Critical</th>
                  <th scope="col">Cases Per One Million</th>
                </tr>
              </thead>
              <tbody>
                <template v-if="isLoadBd">
                  <tr v-if="Object.entries(filterData).length > 0">
                    <td>{{filterData.country}}</td>
                    <td>{{filterData.cases | countFormat}}</td>
                    <td>{{filterData.todayCases | countFormat}}</td>
                    <td class="text-danger">{{filterData.deaths | countFormat}}</td>
                    <td>{{filterData.todayDeaths | countFormat}}</td>
                    <td class="text-success">{{filterData.recovered | countFormat}}</td>
                    <td>{{filterData.active | countFormat}}</td>
                    <td>{{filterData.critical | countFormat}}</td>
                    <td>{{filterData.casesPerOneMillion | countFormat}}</td>
                  </tr>
                  <template v-else>
                    <tr v-for="(countryData, index) in countryDatas.slice(0, 10)" :key="index">
                      <td>{{countryData.country}}</td>
                      <td>{{countryData.cases | countFormat}}</td>
                      <td>{{countryData.todayCases | countFormat}}</td>
                      <td class="text-danger">{{countryData.deaths | countFormat}}</td>
                      <td>{{countryData.todayDeaths | countFormat}}</td>
                      <td class="text-success">{{countryData.recovered | countFormat}}</td>
                      <td>{{countryData.active | countFormat}}</td>
                      <td>{{countryData.critical | countFormat}}</td>
                      <td>{{countryData.casesPerOneMillion | countFormat}}</td>
                    </tr>
                  </template>
                </template>
                <tr v-else>
                  <td colspan="9">
                    <loading :colorName="'text-dark'"></loading>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
    <footer class="py-3 bg-light">
      <div class="container">
        <div class="d-flex justify-content-center">
          <div class="d-inline mr-2">
            Source :
            <a href="https://www.worldometers.info/coronavirus/" target="_blank">Worldometers</a>
          </div>
          <div class="d-inline">
            Developed by
            <a href="https://github.com/rajansarkar" target="_blank">Rajan</a>
          </div>
        </div>
      </div>
      <!-- /.container -->
    </footer>
  </div>
</template>

<script>
import Loading from "../components/Loading";
import axios from "axios";
axios.defaults.baseURL = "https://corona.lmao.ninja/";
export default {
  data() {
    return {
      totalData: {},
      bdtData: {},
      isLoad: false,
      isLoadBd: false,
      countryName: "",
      countryDatas: [],
      filterData: {}
    };
  },

  components: {
    Loading
  },

  filters: {
    countFormat: function(value) {
      return new Intl.NumberFormat().format(value);
    },

    dateFormate(value) {
      var theDate = new Date(value);
      return theDate;
      // return theDate.toGMTString();
    }
  },

  methods: {
    getAllData() {
      axios
        .get("all")
        .then(response => {
          this.totalData = response.data;
          this.getBdtData();
          this.isLoad = true;
        })
        .catch(err => {
          console.log(err.response.data);
        });
    },

    getBdtData() {
      axios
        .get("countries/bangladesh")
        .then(response => {
          this.bdtData = response.data;
          this.getCountry();
          this.isLoadBd = true;
        })
        .catch(err => {
          console.log(err.response.data);
        });
    },

    getCountry() {
      axios
        .get("countries")
        .then(response => {
          this.countryDatas = response.data;
        })
        .catch(err => {
          console.log(err.response.data);
        });
    },

    getDataByCountry() {
      if (this.countryName !== "") {
        axios
          .get("countries/" + this.countryName)
          .then(response => {
            this.filterData = response.data;
          })
          .catch(err => {
            console.log(err.response.data);
          });
      }
    }
  },

  mounted() {
    this.getAllData();
  }
};
</script>