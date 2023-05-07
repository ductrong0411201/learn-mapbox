<template>
  <div class="layout fill-height">
    <div class="overview">
      <div class="overview-bar">
        <OverviewContainer></OverviewContainer>
      </div>
      <div class="overview-body">
        <div class="lot">
          <div class="dashboard">
            <v-card>
              <v-tabs v-model="tab" grow>
                <v-tab href="#tab-1">
                  Green Cover
                </v-tab>

                <v-tab href="#tab-2">
                  Plant Health
                </v-tab>

                <v-tab href="#tab-3">
                  Green Density
                </v-tab>
              </v-tabs>

              <v-tabs-items v-model="tab">
                <v-tab-item :value="'tab-1'">
                  <v-card flat>
                    <GreenCoverVue></GreenCoverVue>
                  </v-card>
                </v-tab-item>
                <v-tab-item :value="'tab-2'">
                  <v-card flat>
                    <PlantHealthVue></PlantHealthVue>
                  </v-card>
                </v-tab-item>
                <v-tab-item :value="'tab-3'">
                  <v-card flat>
                    <PlantDesityVue></PlantDesityVue>
                  </v-card>
                </v-tab-item>
              </v-tabs-items>
            </v-card>
          </div>
          <div class="map">
            <div>
              <!-- <RecordedTrend :resultData="resultData"></RecordedTrend> -->
              <div id="my_dataviz"></div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
  
<script>
import * as d3 from 'd3';
import OverviewContainer from "./OverviewContainer.vue";
import GreenCoverVue from './GreenCover.vue'
import PlantDesityVue from './PlantDesity.vue'
import PlantHealthVue from './PlantHealth.vue'
// import RecordedTrend from './RecordedTrend.vue'
export default {
  components: {
    GreenCoverVue,
    PlantDesityVue,
    PlantHealthVue,
    OverviewContainer,
    // RecordedTrend
  },
  data() {
    return {
      tab: null,
      resultData: null
    }
  },
  async mounted() {
    await this.test()
  },
  methods: {
    async test() {
      const response = await fetch("https://greencover.eofactory.ai/api/v1/imageries/statistics?month=08&compare_month=08&compare_year=2022&year=2022&source=sentinel&overview_type=overall_green_cover&aoi_id=218");
      const jsonData = await response.json();
      jsonData.data.green_cover_change_in_year.green_area['color'] = ['blue']
      jsonData.data.green_cover_change_in_year.green_area = [jsonData.data.green_cover_change_in_year.green_area]
      jsonData.data.green_cover_change_in_year.green_area['group'] = 'Green Cover'
      jsonData.data.green_cover_change_in_year.plant_health['group'] = 'Plant Health'
      jsonData.data.green_cover_change_in_year.green_density['group'] = 'Plant Density'
      var result = [jsonData.data.green_cover_change_in_year.green_area, jsonData.data.green_cover_change_in_year.green_density, jsonData.data.green_cover_change_in_year.plant_health]
      var margin = { top: 10, right: 30, bottom: 20, left: 50 },
        width = 460 - margin.left - margin.right,
        height = 400 - margin.top - margin.bottom;

      // append the svg object to the body of the page
      var svg = d3.select("#my_dataviz")
        .append("svg")
        .attr("width", width + margin.left + margin.right)
        .attr("height", height + margin.top + margin.bottom)
        .append("g")
        .attr("transform",
          "translate(" + margin.left + "," + margin.top + ")");

      var dataTest = result.flat();
      var subgroups = dataTest.map(x => x.label);
      var colorTest = dataTest.map(x => x.color[0]);
      result.forEach(item => {
        item.forEach(e => {
          item[e.label] = e.data[0]
        })
      })

      var groups = result.map(x => x.group)
      // [
      //   "Green Cover",
      //   "Plant Health",
      //   "Plant Density",
      // ]

      // Add X axis
      var x = d3.scaleBand()
        .domain(groups)
        .range([0, width])
        .padding([0.2])
      svg.append("g")
        .attr("transform", "translate(0," + height + ")")
        .call(d3.axisBottom(x).tickSizeOuter(0));

      // Add Y axis
      var y = d3.scaleLinear()
        .domain([0, 350])
        .range([height, 0]);
      svg.append("g")
        .call(d3.axisLeft(y));

      // color palette = one color per subgroup
      var color = d3.scaleOrdinal()
        .domain(subgroups)
        .range(colorTest)

      // stack the data? --> stack per subgroup
      var stackedData = d3.stack()
        .keys(subgroups)(result)
      // Show the bars
      svg.append("g")
        .selectAll("g")
        // Enter in the stack data = loop key per key = group per group
        .data(stackedData)
        .enter().append("g")
        .attr("fill", function (d) { return color(d.key); })
        .selectAll("rect")
        // enter a second time = loop subgroup per subgroup to add all rectangles
        .data(function (d) { return d; })
        .enter().append("rect")
        .attr("x", function (d) {
          return x(d.data.group);
        })
        .attr("y", function (d) { return y(d[1]); })
        .attr("height", function (d) { return (y(d[0]) - y(d[1] ? d[1] : 0)) < 0 ? 0 : (y(d[0]) - y(d[1] ? d[1] : 0)); })
        .attr("width", x.bandwidth())
      // })
    }
  }
}
</script>
  
<style>
.layout {
  background-color: rgb(244, 236, 253);
  padding-bottom: 10px;
}

.button {
  padding: 0px 30px;
  border-top: 2px solid rgb(137, 63, 242);
}

.overview {
  margin: 20px 40px 0px;
  height: calc(100vh - 130px);
  background: rgb(244, 236, 253);
  border-radius: 0px;
  border-radius: 10px;
  border: 1px solid rgba(137, 63, 242, 0.2);
  box-shadow: rgba(0, 0, 0, 0.22) 0px 0px 4px inset;
  background-color: rgb(255, 255, 255);
}

.overview-bar {
  border-top-right-radius: 10px;
  border-top-left-radius: 10px;
  height: 70px;
  border: 1px solid rgba(137, 63, 242, 0.2);
  box-shadow: rgba(0, 0, 0, 0.22) 0px 0px 4px inset;
  background: linear-gradient(rgb(239, 239, 239) 76.97%, rgba(227, 227, 227, 0.28) 100%);
}

.lot {
  height: calc(100vh - 230px);
  border-radius: 10px;
  box-shadow: 0 3px 1px -2px rgba(0, 0, 0, .2), 0 2px 2px 0 rgba(0, 0, 0, .14), 0 1px 5px 0 rgba(0, 0, 0, .12) !important;
  padding: 12px !important;
  margin: 15px;
  display: flex;
}

.dashboard {
  height: 100%;
  width: 50%;
  border-radius: 11px 0px 0px 11px;
  background-color: rgb(247, 243, 248);
}

/* .map{
    height: calc(100% - 5px);
    width: 50%;
  } */
</style>