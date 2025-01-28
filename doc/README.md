# Introduction

DMP Map Components is an (NPM) package containing web frontend components intended to be generally
useful for applications that include a map. Map components integrate with a centralised dataset
catalog and offers the following UI components:

* LayerControl, this controls which layers are shown on the map (danish name Lagvælger or LV)
* DataStore, the datastore is an overview of all the datasets that can be added to the map (danish name Databutik or DB)
* Attribution, this tells the user what the source of the layer is
* LayerToggle, this enables you to turn the layers on or off.

See frontend [components usage](./usage/components.md) to get started.

For more advanced scenarios it's also possible to directly use an [API](./usage/api.md) to access
the dataset catalog without using the components.

# Caveats

- Coordinate system EPSG:25832 is assumed and other projections aren't supported at this time
