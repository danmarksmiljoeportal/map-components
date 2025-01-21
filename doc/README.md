# Introduction

DMP Map Components is an (NPM) package containing web frontend components intended to generally useful for applications that include a map. Map components integrate with a centralised catalog with data sources and offers the following UI components:

* LayerControl, this controls which layers are shown on the map (danish name Lagvælger or LV)
* DataStore, the datastore is an overview of all the datasets that can be added to the map (danish name Databutik or DB)
* Attribution, this tells the user what the source of the layer is
* LayerToggle, this enables you to turn the layers on or off.

See [usage](./usage) for how to get started.

# Caveats

- Coordinate system EPSG:25832 is assumed and other projections aren't supported at this time
