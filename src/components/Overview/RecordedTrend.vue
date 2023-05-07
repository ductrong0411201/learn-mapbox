<template>
    <div>
        <div id="my_dataviz"></div>
    </div>
</template>
<script>
// import { defineComponent } from '@vue/composition-api'
import * as d3 from 'd3';

export default {
    props: ['resultData'],
    mounted() {
        this.barChart()
    },
    methods: {
        barChart() {
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
            console.log(this.resultData)
            var dataTest = this.resultData.flat();
            var subgroups = dataTest.map(x => x.label);
            var colorTest = dataTest.map(x => x.color[0]);
            this.resultData.forEach(item => {
                item.forEach(e => {
                    item[e.label] = e.data[0]
                })
            })

            var groups = [
                "Green Cover",
                "Plant Health",
                "Plant Density",
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
                .keys(subgroups)(this.resultData)
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
        }
    }

}
</script>
<style scoped></style>

