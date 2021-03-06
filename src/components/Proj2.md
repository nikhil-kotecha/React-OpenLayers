```jsx
<Projection2/>
```


```js static
import React, { Component } from "react";

import 'ol/ol.css';
import Map from 'ol/Map';
import View from 'ol/View';
import TileLayer from 'ol/layer/Tile';
import OSM from 'ol/source/OSM';

import {get as getProjection} from 'ol/proj';

import proj4 from 'proj4';
import {register} from 'ol/proj/proj4';

class Projection2 extends Component {

  constructor(props) {
    super(props);

    this.state = { 
        center: [2, 45] , 
        zoom: 3,
        projection: getProjection('EPSG:2154') 
    };

    //déclaration de la projection en EPSG:2154 (Lambert 93)
    this.proj4 =  proj4.defs("EPSG:2154","+proj=lcc +lat_1=49 +lat_2=44 +lat_0=46.5 +lon_0=3 +x_0=700000 +y_0=6600000 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs");
    this.proj4Register = register(proj4);

    this.olmap = new Map({
      target: null,
      layers: [
        new TileLayer({
          source: new OSM()
        })
      ],
      view: new View({
        projection: this.state.projection,  
        center: this.state.center,
        zoom: this.state.zoom
      })
    });
  }

  componentDidMount() {
    this.olmap.setTarget("map3");
  }

  render() {
    return (
      <div id="map3" style={{ width: "100%", height: "360px" }}>
      </div>
    );
  }
}

export default Projection2;
```