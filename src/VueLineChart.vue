<template>
  <div class="svg-line-container" id="bCanvas"></div>
</template>

<script>
import * as d3 from 'd3'
var vm = this;
vm.users = [];
export default {
  name: 'barcharts',
  props: {
    labelOne: {
      default : 'Negativo'
    },
    colorOne: {
      default: '#f90a0a'
    },
    labelTwo: {
      default : 'Positivo'
    },
    colorTwo: {
      default: '#43cc27'
    },
    data: {
      default: function () { 
        return {
        }
      }
    }
  },
  data () {
    return {}
  },
  mounted () {
  console.log('Index.vue');

    // GET /someUrl
    this.$http.get('')
    .then(response=>{
       // get body data
      this.createLine('#bCanvas', response.data, this.labelOne, this.colorOne, this.labelTwo, this.colorTwo);
    }, response=>{
       // error callback
       console.log('error cargando datos');
    })
  },,
  methods: {
    createLine(id, csv, labelOne, colorOne, labelTwo, colorTwo) {
      var canvasWidth = 600
      var canvasHeight = 200
      var margins = {top: 0, bottom: 10, left: 50, rigth: 0}
      var width = canvasWidth - margins.left - margins.rigth
      var height = canvasHeight - margins.top - margins.bottom
      // var width = barHeight * (csv.length - 1)
      var canvas = d3.select(id).append('svg')
        .attr("preserveAspectRatio", "xMinYMin meet")
        .attr("viewBox", "0 0 500 400")
        .classed("svg-content", true)
        .append("svg:g")
      var x = d3.scaleBand()
        .domain(d3.entries(csv).map(function (d) {return d.value.game_name}))
        .rangeRound([0, width]).padding(0.2)
      var y = d3.scaleLinear()
        // .domain([0, d3.max(csv, function (d) {return d.dollars})])
        .domain([0, 100])
        .range([height, 0])
   
      // Define the axes
      var xAx = d3.axisBottom(x).tickSize(0)
      var yAx = d3.axisLeft(y)
      canvas
        .append('g')
        .attr('class', 'spark-y axis')
        .attr("transform", "translate(" + margins.left + ",0)")
        .call(yAx)
      canvas
        .append('g')
        .attr('class', 'spark-x axis')
        .attr("transform", "translate(" + margins.left + "," + height + ")")
        .call(xAx)
      canvas.selectAll(".success").data(d3.entries(csv))
        .enter()
        .append("rect")
        .attr("transform", "translate(" + margins.left + ",0)")
        .attr('class', 'success')
        .attr("x", function (d) { return x(d.value.game_name) })
        .attr("width", x.bandwidth()/2)
        .attr("y", function (d) { return y(d.value.tweets_pos })
        .attr("height", function (d) { return height - y(d.value.tweets_pos) })
        .style( "fill", colorOne )
      var bar = canvas.selectAll(".fail").data(d3.entries(csv))
        .enter()
        .append("rect")
        .attr("transform", "translate(" + margins.left + ",0)")
        .attr("class", "fail")
        .attr("x", function(d) { return x(d.value.game_name) + x.bandwidth()/2 })
        .attr("width", x.bandwidth()/2)
        .attr("y", function(d) { return y(d.value.tweets_neg); })
        .attr("height", function(d) { return height - y(d.value.tweets_neg; })
        .style( "fill", colorTwo )
      canvas
        .append("rect")
        .attr("y", 250)
        .attr("x", 20)
        .attr("transform", "translate(0,-8)")
        .attr("width", 10)
        .attr("height", 10)
        .style( "fill", colorOne )
      canvas
        .append("text")
        .attr("class", "spark-text")
        .attr("y", 250)
        .attr("x", 50)
        .text(labelOne)
        .attr("fill", "black")
      canvas
        .append("rect")
        .attr("y", 250)
        .attr("x", 150)
        .attr("transform", "translate(0,-8)")
        .attr("width", 10)
        .attr("height", 10)
        .style( "fill", colorTwo )
      canvas
        .append("text")
        .attr("class", "spark-text")
        .attr("y", 250)
        .attr("x", 180)
        .text(labelTwo)
        .attr("fill", "black")
    }
  },
  watch: {
    labelOne: {
      handler: function (val, oldVal) {
        d3.select('#bCanvas svg').remove()
        this.createLine('#bCanvas', this.data, val, this.colorOne, this.labelTwo, this.colorTwo)
      }
    },
    colorOne: {
      handler: function (val, oldVal) {
        d3.select('#bCanvas svg').remove()
        this.createLine('#bCanvas', this.data, this.labelOne, val, this.labelTwo, this.colorTwo)
      }
    },
    labelTwo: {
      handler: function (val, oldVal) {
        d3.select('#bCanvas svg').remove()
        this.createLine('#bCanvas', this.data, this.labelOne, this.colorOne, val, this.colorTwo)
      }
    },
    colorTwo: {
      handler: function (val, oldVal) {
        d3.select('#bCanvas svg').remove()
        this.createLine('#bCanvas', this.data, this.labelOne, this.colorOne, this.labelTwo, val)
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.spark {
  stroke: steelblue;
  stroke-width: 1;
}
.svg-line-container {
    display: inline-block;
    position: relative;
    width: 70%;
    padding-bottom: 100%;
    vertical-align: top;
    overflow: visible;
}
.svg-content {
    display: inline-block;
    position: absolute;
    overflow: visible !important;
    top: 0;
    left: 0;
}
.axis {
    shape-rendering: crispEdges;
}
.spark-x line {
  stroke: lightgrey;
}
.spark-x .minor {
  stroke-opacity: .5;
}
.spark-x path {
  display: block;
}
.spark-y line, .spark-y path {
  fill: #999;
 
}
.axis path,
.axis line {
    fill: none;
    stroke: grey;
    stroke-width: 1;
    shape-rendering: crispEdges;
}
</style>