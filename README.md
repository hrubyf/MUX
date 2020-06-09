#  Map User Experience (MUX)
Registrarse en https://code.earthengine.google.com/
## Video 1
a) Seleccionar una imagen especifica de Sentinel
```
var sentinel2 = ee.Image("COPERNICUS/S2/20191217T182751_20191217T183434_T11SNR");
```
b) Ver los detalles de la imagen en la consola
```
print(sentinel2)
```
c) Visualizar la imagen en el mapa
```
Map.addLayer(sentinel2, {bands:["B4","B3","B2"], min:0, max:3000}, "Ensenada - true color");
```



## Video 2
a) Filtrar imagen
```
var sent2_image = ee.Image(sent2_collection
.filterDate("2019-12-01", "2019-12-31")
.filterBounds(ensenada)
.sort("CLOUD_COVERAGE_ASSESSMENT")
.first()
);
```
b) Visualizar la imagen en el mapa
```
Map.addLayer(sent2_image, {bands:["B4","B3","B2"], min:0, max:3000}, "Ensenada - true color");
```
c) Calcular NDVI (https://developers.google.com/earth-engine/tutorial_api_06)
```
var ndvi = sent2_image.normalizedDifference(["B8", "B4"]).rename("NDVI");
```
d) Definir parametros de visualización para el NDVI
```
var ndviParams = {min: -1, max: 1, palette: ["blue", "white", "green"]};
```
e) Visualizar NDVI en el mapa
```
Map.addLayer(ndvi, ndviParams, "NDVI image");
```
## Video 3
a) Filtrar imagen por fecha
```
var time_filter = sent2_collection.filterDate("2019-01-01", "2019-01-03");
Map.addLayer(time_filter, {bands:["B4","B3","B2"], min:0, max:3000}, "Ensenada - true color");
```
b) Generar un mosaico de imagenes
```
Map.addLayer(time_filter.median(), {bands:["B4","B3","B2"], min:0, max:3000}, "Ensenada - median");
```
## Video 4
a) Definir funcion para calcular NDVI para cada imagen de una colección de escenas
```
function add_ndvi_band(image){
var ndvi = image.normalizedDifference(["B8","B4"]);
return image.addBands(ndvi);
}
```
b) Filtrar imagen por fecha
```
var time_filter = sent2_collection.filterDate("2019-12-01", "2019-12-31");
```
c) Ejecutar la función (vease paso a) para cada imagen seleccionado 
```
var time_filter_ndvi = time_filter.map(add_ndvi_band);
```
d) Generar un mosaico del NDVI (a base del median)
```
Map.addLayer(time_filter.median(), {bands:["B4","B3","B2"], min:0, max:3000}, "Ensenada - true color");
Map.addLayer(time_filter_ndvi.median(), {bands:"nd", min: -1, max: 1}, "NDVI");
```
e) Calcular serie de tiempo para el NDVI
```
print(ui.Chart.image.series(time_filter_ndvi.select("nd"),poi));
```
## Video 5
a) Definir parametros para exportar datos como imagen de GEE
```
var exportar_mapa = time_filter_ndvi.median().visualize({bands: ['nd', 'nd', 'nd'],max: 1.0});
```
b) Exportar imagen georreferenciado
```
Export.image.toDrive({image: exportar_mapa, description: 'ndvi',  scale: 10});
```
