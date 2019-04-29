```jsx
<VecteurGeometry />

```

```js static
import React, { Component } from "react";
import Map from 'ol/Map';
import View from 'ol/View';
import TileLayer from 'ol/layer/Tile';
import OSM from 'ol/source/OSM';
import 'ol/ol.css';

import Feature from 'ol/Feature';
import LineString from 'ol/geom/LineString';
import Point from 'ol/geom/Point';
import Polygon from 'ol/geom/Polygon';
import VectorSource from 'ol/source/Vector';
import VectorLayer from 'ol/layer/Vector';

class VecteurGeometry extends Component {
  constructor(props) {
    super(props);

    this.state = { 
        center: [0, 0], 
        zoom: 2 
    };

    // Création de la géometrie de type Point
    this.geomPoint = new Point([2.10,41.23]);
    this.geomPoint.transform('EPSG:4326', 'EPSG:3857');
    // Déclaration de l'objet/entité géographique
    this.entitePoint = new Feature({
        geometry: this.geomPoint,
        name: 'point'
    });

    // Création de la géométrie de type Ligne
    this.geomLigne = new LineString([[2.20,42.23],[2.80,39.23]]);
    this.geomLigne.transform('EPSG:4326', 'EPSG:3857');
    // Déclaration de l'objet/entité géographique
    this.entiteLigne = new Feature({
        geometry: this.geomLigne,
        name: 'ligne'
    });

    //Création de la géométrie de type Polygone
    this.geomPolygone = new Polygon([[[3.0,40.0],[2.90,41.0],[3.50,43.0],[4.0,45.0],[8.0,45.0],[3.0,40.0]]]);
    this.geomPolygone.transform('EPSG:4326', 'EPSG:3857');
    // Déclaration de l'objet/entité géographique
    this.entitePolygone = new Feature({
        geometry: this.geomPolygone,
        name: 'Polygone'
    });

    this.source = new VectorSource({
      features: [this.entitePoint, this.entiteLigne, this.entitePolygone]
    });

    // Déclaration du vecteur
    this.vecteur = new VectorLayer({
      source: this.source
    });

    this.olmap = new Map({
      target: null,
      layers: [
        new TileLayer({
          source: new OSM()
        }),
        this.vecteur
      ],
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
    this.olmap.setTarget("map6");

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
      <div id="map6" style={{ width: "100%", height: "360px" }}>
      </div>
    );
  }
}

export default VecteurGeometry;

```