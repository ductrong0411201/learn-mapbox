<template>
  <div id="app">
    <!--Navbar Here -->
    <div>
      <nav>
        <div class="header">
          <h3>GEOCODER</h3>
        </div>
      </nav>
    </div>
    <!--Index Page Here -->
    
    <!-- huongkoihihi -->
    <div>
      <div class="layout">
        SENTINEL
      </div>
      <div class="overview">
        <div class="bar_overview">
          <div>
            Overview of Changes in
          </div>
          <div>

          </div>
        </div>
        <div class="body_overview">
          <div class="left_overview">
              <div id="my_dataviz"></div>
          </div>
          <index />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import index from "./components/Index.vue";
import * as d3 from "d3";
export default {
  name: "App",
  components: {
    index,
  },
  mounted(){
    this.test();
  },
  methods:{
    test(){
      const margin = {top: 10, right: 30, bottom: 20, left: 50},
      width = 460 - margin.left - margin.right,
      height = 400 - margin.top - margin.bottom;

      // append the svg object to the body of the page
      const data = [
          {
              "group": "banana",
              "Nitrogen": "0",
              "normal": "0",
              "stress": "13"
          },
          {
              "group": "poacee",
              "Nitrogen": "6",
              "normal": "6",
              "stress": "33"
          },
          {
              "group": "sorgho",
              "Nitrogen": "11",
              "normal": "28",
              "stress": "12"
          },
          {
              "group": "triticum",
              "Nitrogen": "19",
              "normal": "6",
              "stress": "1"
          }
      ]
      const svg = d3.select("#my_dataviz")
        .append("svg")
          .attr("width", width + margin.left + margin.right)
          .attr("height", height + margin.top + margin.bottom)
        .append("g")
          .attr("transform", `translate(${margin.left},${margin.top})`);
        // List of subgroups = header of the csv files = soil condition here
        var subgroups = ["Nitrogen", "normal", "stress"]
        // var subgroups = data.columns.slice(1)
        // List of groups = species here = value of the first column called group -> I show them on the X axis
        // var groups = d3.map(data, function(d){return(d.group)}).keys()
        var groups = [
              "banana",
              "poacee",
              "sorgho",
              "triticum"
          ]

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
          .domain([0, 100])
          .range([ height, 0 ]);
        svg.append("g")
          .call(d3.axisLeft(y));

        // color palette = one color per subgroup
        var color = d3.scaleOrdinal()
          .domain(subgroups)
          .range(['#e41a1c','#377eb8','#4daf4a'])

        // Normalize the data -> sum of each group must be 100!
        data.forEach(function(d){
          // Compute the total
          var tot = 0
          for (let i in subgroups){ var name=subgroups[i] ; tot += +d[name] }
          // Now normalize
          for (let i in subgroups){ name=subgroups[i] ; d[name] = d[name] / tot * 100}
        })

        //stack the data? --> stack per subgroup
        var stackedData = d3.stack()
          .keys(subgroups)(data)

        // Show the bars
        svg.append("g")
          .selectAll("g")
          // Enter in the stack data = loop key per key = group per group
          .data(stackedData)
          .enter().append("g")
            .attr("fill", function(d) { return color(d.key); })
            .selectAll("rect")
            // enter a second time = loop subgroup per subgroup to add all rectangles
            .data(function(d) { return d; })
            .enter().append("rect")
              .attr("x", function(d) { return x(d.data.group); })
              .attr("y", function(d) { return y(d[1]); })
              .attr("height", function(d) { return y(d[0]) - y(d[1]); })
              .attr("width",x.bandwidth())

    }
  }
};
</script>

<style>
.left_overview{
  width: 50%;
  margin: 20px 20px;
  padding:10px;
  /* height: 100%;
  width: 50%; */
  /* border: 1px solid rgba(137, 63, 242, 0.2); */
  background-color: rgb(244, 239, 250);
  border-radius: 11px 0px 0px 11px;
}
.body_overview{
  margin: 10px;
  border-radius: 10px;
  box-shadow: 0 3px 1px -2px rgba(0,0,0,.2),0 2px 2px 0 rgba(0,0,0,.14),0 1px 5px 0 rgba(0,0,0,.12)!important;
  display: flex;
}
.bar_overview{
  border-top-right-radius: 10px;
  border-top-left-radius: 10px;
  height: 70px;
  border: 1px solid rgba(137, 63, 242, 0.2);
  box-shadow: rgba(0, 0, 0, 0.22) 0px 0px 4px inset;
  background: linear-gradient(rgb(239, 239, 239) 76.97%, rgba(227, 227, 227, 0.28) 100%);
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 20px;
  color: rgba(0,0,0,.6);
}
.overview{
  margin: 0px 40px;
  border-radius: 10px;
  height: 100%;
  border: 1px solid rgba(137, 63, 242, 0.2);
  box-shadow: rgba(0, 0, 0, 0.22) 0px 0px 4px inset;
  background-color: rgb(255, 255, 255);
}
.layout {
  display: flex;
  flex: 1 1 auto;
  flex-wrap: nowrap;
  min-width: 0;
}
#app {
  background-color:rgb(244, 239, 250);
  color: rgb(137, 63, 242);
  font-family: system-ui, -apple-system, Roboto sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
html,
body {
  margin: 0;
}

nav {
  box-shadow: 0px 2px 5px rgba(0, 58, 78, 0.15);
}
.header {
  display: flex;
  flex-direction: row;
  align-items: center;
  padding: 0px 50px;
}

.header h3 {
  font-weight: 600;
}
</style>
