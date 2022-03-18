<template>
<div>
  <div class="container">
    <div class="row mb-4" >
      <div class="col col-lg-6">
        <h5 class="text-start">
          Seleccione vehiculo
        </h5>
        <select class="form-select" v-model="vehiculoSeleccionado">
            <option :value="this.vehiculoSeleccionado" disabled hidden>Seleccione un vehiculo</option>
            <option  v-for="vehiculo in vehiculos" :value="vehiculo" v-bind:key="vehiculo.idvehiculo">
            {{ vehiculo.marca }} {{vehiculo.modelo}} ( {{vehiculo.patente}} )
            </option>

        </select>
      </div>
      <div class="col col-lg-6 d-flex align-items-end text-center ">
        <!-- v-if="vehiculoSeleccionado!=null" -->
        <button  :disabled="vehiculoSeleccionado == null" type="button" class="btn btn-danger" v-on:click.once="mostrarMovimientos">Movimientos Online</button>
      </div>
    </div>
    <transition name="slide-fade">
      <div class="row" v-if="show">
        <div class="col mb-4">
          <div class="card">
            <div class="card-body">
              <h6 class="card-subtitle mb-2 text-muted">Recorrido del vehiculo</h6>
              <div class="text-center align-items-center d-flex justify-content-center">
                <LMap :zoom="zoom" :center="center" style="height: 300px; width: 550px">
                  <LTileLayer :url="url"></LTileLayer>
                  <LMarker v-for="marker in markers" :lat-lng="marker.marker" v-bind:key="marker">
                    <l-icon
                      :icon-size="dynamicSize"
                      :icon-anchor="dynamicAnchor"
                      icon-url= "/iconCar.png"
                  />
                  <l-tooltip>{{marker.evento.descripcion}} - {{marker.horarioEvento}}</l-tooltip>
                  </LMarker>
                  <l-polyline :lat-lngs="polyline.latlngs" :color="polyline.color"></l-polyline>
                  
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
    </transition>
  </div> 
</div>
    
</template>

<script>
import L from 'leaflet';
import { LMap, LTileLayer, LMarker ,LPolyline , LIcon , LTooltip} from "vue2-leaflet";
import json from './../assets/ejercicioMovimientoOnline.json'
delete L.Icon.Default.prototype._getIconUrl;
export default {
  name: "MapVisual2",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPolyline,
    LIcon,
    LTooltip
  },
  data() {
    return {
      show: false,
      selected:null,
      eventosOcurridos:json,
      vehiculos:[],
      vehiculoSeleccionado:null,
      url: "https://{s}.tile.osm.org/{z}/{x}/{y}.png",
      zoom: 7,
      center: [-38.94351,-67.97904],
      polyline: {
        // latlngs: [[47.334852, -1.509485], [47.342596, -1.328731], [47.241487, -1.190568], [47.234787, -1.358337]],
        latlngs: [],
        color: '#dc3545'
      },
      bounds: null,
      icon: L.icon({
        iconUrl: 'http://icons.iconarchive.com/icons/paomedia/small-n-flat/1024/map-marker-icon.png',
        iconSize: [32, 37],
        iconAnchor: [16, 37]
      }),
      staticAnchor: [16, 37],
      customText: 'Foobar',
      iconSize: 35,
      markers:[],
      eventosPosibles:[
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
  computed:{
    //--------metodos para cambiar dinamicamente el tamaño del icono marker--------
    dynamicSize () {
      return [this.iconSize, this.iconSize * 1.15];
    },
    dynamicAnchor () {
      return [this.iconSize / 2, this.iconSize * 1.15];
    }
  },
  methods: {
    obtenerVehiculos(){
      //---------obtengo los vehiculos para mostrarlos en el select--------
      var primerVehiculo=this.eventosOcurridos[0];
      this.vehiculos.push(primerVehiculo);
      this.eventosOcurridos.forEach(eventoOcurrido => {
        if(eventoOcurrido.idvehiculo!=primerVehiculo.idvehiculo){
          this.vehiculos.push(eventoOcurrido);
        }
      });
    },  
    mostrarMovimientos() {
      //----muestro el mapa e informacion del vehiculo------
      this.show=true;
      var eventosVehiculo=[];
      //--------obtengo los eventos ocurridos del vehiculo seleccionado-----------
      this.eventosOcurridos.forEach(eventoOcurrido => {
        if(eventoOcurrido.idvehiculo==this.vehiculoSeleccionado.idvehiculo){
          eventosVehiculo.push(eventoOcurrido);
        }
      });
      //------ordeno los eventos del mas antiguo al mas reciente--------------
      eventosVehiculo.sort((a, b) => new Date(b.fechayhora) - new Date(a.fechayhora));
      console.log("eventos vehiculo ordenados",eventosVehiculo);

      //------recorro los eventos del vehiculo , guardo en arrays los marcadores , las polilineas y como marcador central el primer evento
      eventosVehiculo.forEach(eventoVehiculo => {
        if(eventoVehiculo.idvehiculo==this.vehiculoSeleccionado.idvehiculo){
          var evento=this.eventosPosibles.find(element => element.idevento == eventoVehiculo.idevento)
          this.markers.push(
            {"marker":[Number(eventoVehiculo.latitud),Number(eventoVehiculo.longitud)],
            "evento":evento,
            "horarioEvento":eventoVehiculo.fechayhora}
          );
          this.polyline.latlngs.push([Number(eventoVehiculo.latitud),Number(eventoVehiculo.longitud)]);
          this.center=this.markers[this.markers.length-1].marker;
        }
      });
    console.log("selecciono algo");
  }
  },
  mounted() {
      this.obtenerVehiculos();
      console.log("vehiculos",this.vehiculos);
      console.log("eventos ocurridos",this.eventosOcurridos);
      
  }
}

</script>
<style>
.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}
</style>
