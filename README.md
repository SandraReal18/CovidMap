# data-visualization-covid-map

## GOALS:

#### Our goal is from https://github.com/Lemoncode/d3js-typescript-examples/tree/master/02-maps/02-pin-location-scale to implement a button to change the covid data used in that repository and current covid data.

## STEPS:

#### 
 - The first thing we have had to do is look for the data at https://datos.gob.es/ (With the help of Francisco Florido). We have added this data to _'stats.ts'_
- In the _index.html_ we had to add the following:
```typescript
 <link rel="stylesheet" type="text/css" href="./style.css" />
```
-  We have removed the constants _maxAffected_ and _affectedRadiusScale_ from the code since we were not going to use them. (_index.ts_)
- We have created a helper function _calculateBasedOnAffectedCases_. Which calculates the maximum number of cases affected by covid and then scaling the data by making a rule of three. (_index.ts_)
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
```
- The _calcalculateRadiusBasedOnAffectedCases_ function pastes the community name with the value of scaled affected cases. (_index.ts_)
```typescript
const calculateRadiusBasedOnAffectedCases = (
  comunidad: string,
  data: any[]
) => {
  return calculateBasedOnAffectedCases(comunidad, data);
};
```
- To create the circles we have created the constant _updateChart_. It paints the circles in each community with the scaled radius thanks to the two functions that we have created previously. (with the help of Álvaro Jiménez Balmisa) (_index.ts_)
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
## INSTRUCTIONS TO START IT

#### In order to start it we have to first execute these commands in the terminal to install the packages that we will use:
 -  _npm install_
 -  _npm install d3-composite-projections --save_ 
 -  _npm install @types/node --save-dev_ 
 -  _npm install @types/topojson-client --save-dev_      

 #### Once these installations are done to start we have to execute the _npm start_ command and access the localhost to be able to view the results.