# data-visualization-covid-map

### GOALS:

- Place pins on a map based on location.
- Scale pin radius based on affected number.
- Spain got canary island that is a territory placed far away, we need to cropt that islands and paste them in a visible place in the map.
- Create a button to update the information of the two datasets.

Our starting point will be from the following repository. https://github.com/Lemoncode/d3js-typescript-examples/tree/master/02-maps/02-pin-location-scale

### STEPS:

#### 
 - The first thing we have had to do is look for the data at https://datos.gob.es/ (With the help of Francisco Florido). We have added this data to _'stats.ts'_
- In the _index.html_ we had to add the following:
```typescript
 <link rel="stylesheet" type="text/css" href="./style.css" />
```
-  We have removed the constants _maxAffected_ and _affectedRadiusScale_ from the code since we were not going to use them. (_index.ts_)
- We have created two helper function _calculateBasedOnAffectedCases_ and  _calcalculateRadiusBasedOnAffectedCases_. Where the maximum affected is calculated, a radius scale is created for the radius of each circumference of the community according to the number of cases in the most affected community, which depends on the number of affected. (_index.ts_)
```typescript
    const calculateBasedOnAffectedCases = (comunidad: string, data: any[]) => {
  const entry = data.find((item) => item.name === comunidad);
  var max = data[0].value;
  for (var i = 0; i < data.length; i++) {
    if (max < data[i].value) {
      max = data[i].value;
    }
  }
  return entry ? (entry.value / max) * 50: 0;
};

const calculateRadiusBasedOnAffectedCases = (
  comunidad: string,
  data: any[]
) => {
  return calculateBasedOnAffectedCases(comunidad, data);
};
```
- To update the graph when the buttons are clicked for this we have implemented the following function (with the help of Álvaro Jiménez Balmisa) (_index.ts_)
```typescript
const updateChart = (data: any[]) => {
  svg.selectAll("circle").remove();
  svg
    .selectAll("circle")
    .data(latLongCommunities)
    .enter()
    .append("circle")
    .attr("class", "affected-marker")
    .attr("r", (d) => calculateRadiusBasedOnAffectedCases(d.name, data))
    .attr("cx", (d) => aProjection([d.long, d.lat])[0])
    .attr("cy", (d) => aProjection([d.long, d.lat])[1]);
};

```
- To implement the buttons, we first added in the _index.ts_
```typescript
 document
  .getElementById("Previous")
  .addEventListener("click", function handleResultsApril() {
    updateChart(stats_0);
  });

document
  .getElementById("Actual")
  .addEventListener("click", function handleResultsNovember() {
    updateChart(stats_1);
  });
 ```
Then we had to add the following in _index.html_:
```typescript
<body>
    <button id="Previous">Resultados anteriores</button>
    <button id="Actual">Resultados Actuales</button>
    <script src="./index.ts"></script>
</body>
```
### INSTRUCTIONS TO START IT

In order to start it we have to install npm:
 -  _npm install_
We install the package to be able to work with maps, the library that contains this projections and the node typings to get require typing
 -  _npm install @types/topojson-client --save-dev_ 
 -  _npm install d3-composite-projections --save_ 
 -  _npm install @types/node --save-dev_ 
      

 #### Once these installations are done to start we have to execute the _npm start_ command and access the localhost to be able to view the results.
