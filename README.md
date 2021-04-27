# Tarea Advanced 

## Visualización de Datos con Visual Studio Code

- Colorearemos el mapa de España, diferenciando las Comunidades Autónomas según su grado de incidencia estará determinada por el color, asimismo seguimos introduciendo los círculos, pero gracias a colorear las provincias, será más visual el grado de incidencia:

```sh
const color = d3
  .scaleThreshold<number, string>()
  .domain([0, 10, 20, 30, 40, 50, 150, 200, 500, 800, 3000])
  .range([
    "#FFFFF",
    "##FFE8E5",
    "#D09F9A",
    "#C6B0AE",
    "#FFE8E5",
    "#F88F70",
    "#CD6A4E",
    "#A4472D",
    "#7B240E",
    "#540000",
    "#57423F",
  ]);
```

- Ralizamos una implicación directa entre el color y grado de incidencia en cada Comunidad Autónoma:

```
const assignCommunityBackgroundColor = (
  comunidad: string,
  stats: ResultEntry[]
) => {
  const item = stats.find((item) => item.name === comunidad);
  return item ? color(item.value) : color(0);
};
```

- Para la comparación entre los datos iniciales y actuales introducimos dos botones, tambien tenemos que fijar que cambien los colores de las comunidades autónomas en la comparación:

```
const updateChart = (stat: ResultEntry[]) => {
  console.log("updating");
  svg.selectAll("path").remove();
  svg.selectAll("circle").remove();
  svg
    .selectAll("path")
    .data(geojson["features"])
    .enter()
    .append("path")
    .attr("class", "country")
    .attr("d", geoPath as any)
    .style("fill", function (d: any) {
      return assignCommunityBackgroundColor(d.properties.NAME_1, stat);
    });
  svg
    .selectAll("circle")
    .data(latLongCommunities)
    .enter()
    .append("circle")
    .attr("class", "affected-marker")
    .attr("r", (d) => calculateRadiusBasedOnAffectedCases(d.name, stat))
    .attr("cx", (d) => aProjection([d.long, d.lat])[0])
    .attr("cy", (d) => aProjection([d.long, d.lat])[1]);
};
```
