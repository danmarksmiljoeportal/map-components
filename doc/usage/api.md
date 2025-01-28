# API usage

As an alternative to accessing the dataset catalog service directly a JavaScript/TypeScript API has been developed. It is the same logic that has been used to implement [LayerControl](#layercontrol) and the [DatasetStore](#datasetstore). The API models the data in the catalog service and handles all requests for the service. Furthermore it includes several helper functions that can be used creating applications. 

See [API docs](https://npmdoc.miljoeportal.dk/map-components/) for general technical reference.

Basic use of the API:

```javascript
import { Api } from '@dmp/map-components'
const api = new Api()
```

The API can be used with or without a map, however it is normally used in combination with a map. A map can be created using a variety of map libraries. The most common is [OpenLayers](https://openlayers.org/) and the API by default integrates to OpenLayers. Other libraries, like [MapLibre](https://maplibre.org/) can be used, but you need to do more of the implementation yourself.

### Options

To control the initial state of the datasets and how they are presented, use the following options.

#### onlyRenderable

The Datacatalog contains some datasets, that are not renderable, like zip-file. If the API is purely used for map rendering, you need to add the `onlyRenderable` when instantiating the API like this:

```javascript
import { Api } from '@dmp/map-components'

const api = new Api({
  onlyRenderable: true,
})
```
Then only renderable datasets will be available to the user.

#### locale

By default the locale is `dk-DK` but it is possible to get the metadata and the components in `en-US` like this:

```javascript
const api = new Api({
  locale: 'en-US',
})
```

Furthermore you can dynamically change the locale with:

```javascript
api.setLocale('en-US')
```

#### datasetState

The default active datasets are defined by adding a `datasetState` like this:

```javascript
import { Api } from '@dmp/map-components'

const api = new Api({
  onlyRenderable: true,
  datasetState: [
    { 
      id: 'urn:dmp:ds:skaermkort-daempet', 
      visible: true, 
      opacity: 0.5,
    }
  ],
})
```

Call the `api.load()` method to initialize the state of the active datasets. Changes to the active datasets are stored in local storage in the browser. By calling `load` witout arguments, local storage is read and used as current datasetState:
```javascript
api.load()
```

If you want a specifik state and ignore the datasetState in the local storage in the browser, add the `datasetState` as the argument to the load method like this:
```javascript
api.load([
  { 
    id: 'urn:dmp:ds:skaermkort-daempet', 
    visible: true, 
    opacity: 0.5,
  }
])
```

### OpenLayers

To use the API with [OpenLayers](https://openlayers.org/), the active datasets can be added to the map with:
```javascript
import Map from 'ol/Map'
import View from 'ol/View'
import { Api, projections } from '@dmp/map-components'

const api = new Api({
  onlyRenderable: true,
})

const map = new Map({
  target: 'map',
  layers: [api.getOlGroup()],
  view: new View({
    center: [601283, 6206304],
    zoom: 3,
    projection: projections[25832].projection,
  }),
})

map.addLayer(api.getOlGroup())
```

The `layerGroup` is a collection that the API is maintaining. By adding the layers as a group, the API can change and reorder the internal layers as needed.

On each dataset, there are a `getOlLayer` method that will create an OpenLayers layer. This can be used if you are creating your own layer control or a more specific map like an overview map.

### Events

If the application need to know when something changes in the API, there at multiple event to listen to. See the API docs for more details.

### Query

The API contains functionality to query a single dataset or a liste of datasets. This can be used for something like click in the map to show information about the datasets visible in that location. But it can also be used for other kinds of quering.

To query all visible datasets by a specific coordinate, use somthing like this:

```javascript
  const promises = api.queryByCoordinate(coordinate, undefined, {
    buffer: 1,
    resolution: map.getView().getResolution()
  })
  const result = await Promise.all(promises)
```
Other query methods can be used like `queryByExtent` or the full flexible `query` for queriing with more advanced filters, both spatial and/or attribute filters.

A `query` can be found on a dataset as well.

By using this functionality, you don't need to know anything about the datasource and how to make the request.

### Download

The API contains a helper function that makes it possible to download a list of datasets as a QGIS project. This makes it easier to continue work in a desktop application. In your application add something like this:

```javascript
import { saveToQgs } from '@dmp/map-components'

saveToQgs({ 
  name: 'download',
  datasets,
})
```
