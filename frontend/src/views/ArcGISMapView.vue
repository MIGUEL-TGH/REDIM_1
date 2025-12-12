<template>
  <div>
    <v-app-bar app dark elevation="10" color="#ec1e4c" clipped-left>
      <v-app-bar-nav-icon @click.stop="drawer_left_map=!drawer_left_map"></v-app-bar-nav-icon>
        logo 1
      <v-spacer></v-spacer>
        <img src="@/assets/logos/redim_logo.png" style="width: 100%; max-width: 60px; height: auto;">
    </v-app-bar>
    <div id="viewDiv" ref="mapView"></div>
    <v-navigation-drawer app dark v-model="drawer_left_map" width="250px" clipped style="padding-top: 10px;" color="#B0B0B0">
      Navegar por el mapa
    </v-navigation-drawer>
    <!-- #3c8c43 #4071b2-->
  </div>
</template>

<script>
import '@/assets/css/style_maps.css'
import '@arcgis/core/assets/esri/themes/light/main.css'
import Map from '@arcgis/core/Map'
import MapView from '@arcgis/core/views/MapView'
import * as reactiveUtils from '@arcgis/core/core/reactiveUtils'
// import Graphic from '@arcgis/core/Graphic'
// import GraphicsLayer from '@arcgis/core/layers/GraphicsLayer'
// import Point from '@arcgis/core/geometry/Point'

export default {
  name: 'ArcGISMap',
  components: { // Importación de componentes hijos
  },
  directives: {}, // Directivas personalizadas
  filters: {}, // Filtros (si usas)

  // 2️⃣ Propiedades de entrada
  props: {},
  mixins: {},
  extends: {},

  // 3️⃣ Datos reactivas
  data () {
    return {
      // map
      vector: [
        'streets-navigation-vector',
        'streets',
        'topo-vector',
        'satellite',
        'hybrid',
        'dark-gray-vector',
        'oceans',
        'national-geographic',
        'terrain',
        'satellite'
      ],
      view: undefined,
      map: undefined,
      stopWatchHandle: null,
      graphicsLayer: null,
      // vue
      drawer_left_map: false
    }
  },
  computed: {

  },

  // 4️⃣ Observadores
  watch: {},

  // 5️⃣ Métodos
  methods: {
    // map
    async initMap () {
      console.log('initMap()')
      this.map = new Map({
        basemap: this.vector[0] // 'streets-navigation-vector'
      })

      this.view = new MapView({
        container: this.$refs.mapView,
        map: this.map,
        // center: [-98.18635039767328, 19.054906905810686], // Estadio Olímpico Zaragoza
        center: [-102.37592182483502, 24.097127823504444], // Estadio Olímpico Zaragoza
        // 24.097127823504444, -102.37592182483502
        zoom: 5
      })

      await this.view.when() // Esperar a que la vista esté lista antes de agregar el componente

      this.stopWatchHandle = reactiveUtils.watch(
        () => this.view.updating, // 🔹 propiedad reactiva
        async (updating) => {
          if (!updating) {
            // await this.setSleep(1500) // 🔸 esperamos un poco para asegurar que todo terminó de renderizar
            // this.dialog_loader.actived = false
            // this.btnGet = false
          }
        }
      )
    }
  },

  // 6️⃣ Ciclo de vida
  beforeCreate () {},
  created () {
  },
  beforeMount () {},
  mounted () {
    this.initMap()
  },
  beforeUpdate () {},
  updated () {},
  beforeDestroy () {
  },
  destroyed () { }

  // 7️⃣ Hooks específicos (como de rutas, etc.)
  // beforeRouteEnter() {}, etc.

}
</script>

<style scoped></style>
