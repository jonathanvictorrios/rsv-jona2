<template>
<div>
  <div class="container">
    <div class="row mb-4">
      <div class="col">
        <h5 class="text-start">
          Seleccione vehiculo
        </h5>
        <select class="form-select" v-model="vehiculoSeleccionado" @change="onChange">
            <option  v-for="vehiculo in vehiculos" :value="vehiculo" v-bind:key="vehiculo.idvehiculo">
            {{ vehiculo.marca }} {{vehiculo.modelo}} ( {{vehiculo.patente}} )
            </option>
        </select>
      </div>
    </div>
    <div class="row">
      <div class="col mb-4">
        <div class="card">
          <div class="card-body">
            <h6 class="card-subtitle mb-2 text-muted">Ubicacion del vehiculo</h6>
            <div class="text-center align-items-center d-flex justify-content-center">
              <LMap :zoom="zoom" :center="center" style="height: 300px; width: 550px">
                <LTileLayer :url="url"></LTileLayer>
                <LMarker v-for="marker in markers" :lat-lng="marker" v-bind:key="marker"></LMarker>
              </LMap>
            </div>
          </div>
        </div>
      </div>
      <div class="col mb-4">
        <div class="card p-0">
          <div class="card-body ">
              <h6 class="card-subtitle mb-2 text-muted">Informacion del vehiculo</h6>
              <div class="text-center">
                <ul v-if="vehiculoSeleccionado!=null" class="list-group list-group-flush">
                  <li class="list-group-item">
                    <label class="form-check-label text-muted" for="exampleCheck1">Interno:</label>
                    {{vehiculoSeleccionado.interno}}
                  </li>
                  <li class="list-group-item">
                    <label class="form-check-label text-muted" for="exampleCheck1">Marca:</label>
                    {{vehiculoSeleccionado.marca}}
                    </li>
                  <li class="list-group-item">
                    <label class="form-check-label text-muted" for="exampleCheck1">Modelo:</label>
                    {{vehiculoSeleccionado.modelo}}
                    </li>
                  <li class="list-group-item">
                    <label class="form-check-label text-muted" for="exampleCheck1">Conductor:</label>
                    {{vehiculoSeleccionado.conductor}}

                  </li>
                  <li class="list-group-item">
                    <label class="form-check-label text-muted" for="exampleCheck1">Fecha y hora:</label>
                    {{vehiculoSeleccionado.fechayhora}}
                  </li>
                  <li class="list-group-item">
                    <label class="form-check-label text-muted" for="exampleCheck1">Localizacion:</label>
                    {{vehiculoSeleccionado.localizacion}}
                  </li>
                  <li class="list-group-item">
                    <label class="form-check-label text-muted" for="exampleCheck1">Id gps:</label>
                    {{vehiculoSeleccionado.idgps}}
                  </li>
                  <li class="list-group-item">
                    <label class="form-check-label text-muted" for="exampleCheck1">Evento:</label>
                    {{eventoSelecionado}}
                  </li>
                </ul>
                <p v-else>
                  ......................
                  <br>
                  ..................
                  <br>
                  ..............
                  <br>
                  ........
                </p>
              </div>
          </div>
        </div>
      </div>
    </div>
  </div> 
</div>
    
</template>

<script>
import { LMap, LTileLayer, LMarker } from "vue2-leaflet";
import json from './../assets/ejercicioPosiciones.json'
export default {
  name: "MapVisual2",
  components: {
    LMap,
    LTileLayer,
    LMarker
  },
  data() {
    return {
      selected:null,
      vehiculos:json,
      vehiculoSeleccionado:null,
      url: "https://{s}.tile.osm.org/{z}/{x}/{y}.png",
      zoom: 7,
      center: [-38.94351,-67.97904],
      bounds: null,
      markers:[],
      eventos:[
      {"idevento":1,"descripcion":"Actividad"},
      {"idevento":2,"descripcion":"Ignicion"},
      {"idevento":3,"descripcion":"Ralenti"},
      {"idevento":4,"descripcion":"Parada"},
      {"idevento":5,"descripcion":"Exceso"},
      {"idevento":6,"descripcion":"Fin Exceso"},
      {"idevento":7,"descripcion":"Estacionado"},
      {"idevento":8,"descripcion":"Remolcado"},
      {"idevento":9,"descripcion":"Actividad"},
      {"idevento":10,"descripcion":"Fin Exceso"},
      {"idevento":11,"descripcion":"Frenada"},
      {"idevento":21,"descripcion":"Actividad 2"},
      {"idevento":81,"descripcion":"Panico"}
      ],
      eventoSelecionado:null

    };
  },
  methods: {
     onChange() {
      this.markers=[[Number(this.vehiculoSeleccionado.latitud), Number(this.vehiculoSeleccionado.longitud)]];
      var i = 0;
      var encontro=false;
      var cantEventos=Object.keys(this.eventos).length;
      console.log(cantEventos);
      while (i < cantEventos && encontro==false) {
        if(this.eventos[i].idevento==this.vehiculoSeleccionado.idevento){
          encontro=true;
          this.eventoSelecionado=this.eventos[i].descripcion;
        }
        i ++;
      }
      console.log("selecciono algo");
    }
  },
  mounted() {
      console.log(this.vehiculos);
      console.log(this.eventos)
  }
}
</script>
<style>

</style>