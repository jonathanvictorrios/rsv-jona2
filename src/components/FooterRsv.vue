<template>
<div class="bg-dark mb-0">
  <div class="container text-white">
    <div class="row">
      <div class="col text-end mt-2">
        {{nombre}}
      </div>
      <div class="col text-start mt-2">
        {{info}}
      </div>
    </div>
    <div class="row">
      <div class="col text-end">{{anio}}</div>
      <div class="col"></div>
    </div>
    <div class="row">
      <div class="col mb-2">
        {{email}}
      </div>
    </div>
   </div>
</div>
</template>
    
<script>
export default {
    name: "FooterRsv",
    data(){
      return {
        nombre:"Jonathan Victor Rios",
        email: "victor.rios@est.fi.uncoma.edu.ar",
        info: "Ejercicio para rsv",
        anio: "2022"
      }
    }
}
</script>