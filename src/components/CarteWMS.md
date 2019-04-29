```jsx
<CarteWMS />
```

```js static
import React, { Component } from "react";
import Map from 'ol/Map';
import View from 'ol/View';
import 'ol/ol.css';

import TileWMS from 'ol/source/TileWMS';
import TileLayer from 'ol/layer/Tile';
import ImageWMS from 'ol/source/ImageWMS';
import ImageLayer from 'ol/layer/Image';

class CarteWMS extends Component {
  constructor(props) {
    super(props);

    this.state = { 
        center: [-8233510, 4980620], 
        zoom: 5 
    };

    // Déclaration de la source de la couche en format WMS Tuilé 
    this.sourceWMSTile = new TileWMS({
	// Chargement du lien WMS 
	url: 'https://map.geomatick.com/geoserver/wms',
	// Chargement de l'espace de travail : couche 
	params: {'LAYERS': 'topp:states', 'TILED': true},
	serverType: 'geoserver'
    })
    // Déclaration de la couche WMS Tuilé
    this.coucheWMTile = new TileLayer({ 
	source: this.sourceWMSTile,
	opacity: 0.5
    });

    // Déclaration de la source de la couche en format WMS Image
    this.sourceWMSImage = new ImageWMS({
	// Chargement du lien WMS 
	url: 'https://map.geomatick.com/geoserver/wms',
	// Chargement de l'espace de travail : couche 
	params: {'LAYERS': 'tiger:poly_landmarks'},
	serverType: 'geoserver'
    });
    // Déclaration de la couche WMS 
    this.coucheWMSImage = new ImageLayer({ source: this.sourceWMSImage });

    this.olmap = new Map({
      target: null,
      layers: [this.coucheWMTile,this.coucheWMSImage ],
      view: new View({
        center: this.state.center,
        zoom: this.state.zoom
      })
    });
  }

  updateMap() {
    this.olmap.getView().setCenter(this.state.center);
    this.olmap.getView().setZoom(this.state.zoom);
  }

  componentDidMount() {
    this.olmap.setTarget("map9");

    // Listen to map changes
    this.olmap.on("moveend", () => {
      let center = this.olmap.getView().getCenter();
      let zoom = this.olmap.getView().getZoom();
      this.setState({ center, zoom });
    });
  }

  shouldComponentUpdate(nextProps, nextState) {
    let center = this.olmap.getView().getCenter();
    let zoom = this.olmap.getView().getZoom();
    if (center === nextState.center && zoom === nextState.zoom) return false;
    return true;
  }


  render() {
    this.updateMap(); // Update map on render?
    return (
      <div id="map9" style={{ width: "100%", height: "360px" }}>
      </div>
    );
  }
}

export default CarteWMS;
```