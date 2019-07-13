<template>
  <div class="c-map">
    
    <div style="display: flex;" >
      <div class="c-map__container">
        <div class="c-map__search">
          <input
            type="text" 
            placeholder="Find nearby restaurants" 
            v-model="search"/>
        </div>

        <GmapMap
          :center="myPos"
          :zoom="12"
          :options="{
            zoomControl: false,
            mapTypeControl: false,
            scaleControl: false,
            streetViewControl: false,
            rotateControl: false,
            fullscreenControl: false,
            disableDefaultUi: true
          }"
          map-type-id="terrain"
          style="width: 100%; height: 100%">

          <GmapMarker 
            :position="myPos"
            
          /> 
          
          <GmapMarker
            :key="index"
            v-for="(m, index) in markers"
            :position="m.position"
            :clickable="true"
            :draggable="false"
            @click="toggleInfoWindow(m,index)"
          />
          
          <GmapInfoWindow
            :options="infoOptions" 
            :position="infoWindowPos" 
            :opened="infoWinOpen" 
            @closeclick="infoWinOpen=false">
            <p class="infoWindow__item name">{{ infoContent.name }}</p>
            <p class="infoWindow__item name">{{ infoContent.range }}</p>
            <p class="infoWindow__item name">{{ infoContent.discount }}</p>
            <p class="infoWindow__item name">{{ infoContent.meals }}</p>
            <p class="infoWindow__item name">{{ infoContent.food }}</p>
            <p class="infoWindow__item name">{{ infoContent.closing }}</p>
          </GmapInfoWindow>
        </GmapMap>
        
        <div class="c-map__footer" v-if="!desktop">
          <div class="c-map__footer-item active">Explore</div>
          <div class="c-map__footer-item">Profile</div>
          <div class="c-map__footer-item">Payment</div>
        </div>
      </div>

      
        
      <div v-if="desktop">
        <div class="c-map__info"></div>
      </div>

       
    
    </div>
   <!--
    <ul v-if="desktop">
      <li :key="index"
          v-for="(m, index) in markers">
          name: {{ m.name }}
          pos: {{ m.position }}
      </li>
    </ul> -->
      

  </div>
</template>

<script>
//import GmapMap from 'vue2-google-maps'
//import GmapMarker from 'vue2-google-maps'
import * as VueGoogleMaps from 'vue2-google-maps'
import iconCurrent from '@/assets/img/latebirdicon.png'


export default {
  name: 'Map',
  props: {
    msg: String
  },
  data: () => ({
    desktop: false,
    search: '',
    icons: {
      current: iconCurrent
    },
    infoContent: {
      name: '',
      range: '',
      discount: '',
      meals: '',
      food: '',
      closing: '',
    },
    infoWindowPos: null,
    infoWinOpen: false,
    currentMidx: null,
    //optional: offset infowindow so it visually sits nicely on top of our marker
    infoOptions: {
      pixelOffset: {
        width: 0,
        height: -35
      }
    },
    myPos: {
      lat: Number,
      lng: Number
    },
    markers:[
      {
        name: "Café Sunday",
        range: "$$",
        discount: "50%",
        meals: 10,
        food: "Café",
        closing: "4:30 PM",
        position: {
          lat: -33.8966662,
          lng: 151.25162289999998
        },
      },
      {
        name: "Sushi House",
        range: "$",
        discount: "60%",
        meals: 15,
        food: "Sushi",
        closing: "6:00 PM",
        position: {
          lat: -33.9166662,
          lng: 151.19062289999998
        },
      },
      {
        name: "The Freeranger",
        range: "$$",
        discount: "75%",
        meals: 20,
        food: "Fast food/restaurant",
        closing: "5:00 PM",
        position: {
          lat: -33.8366662,
          lng: 151.22962289999998
        },
      },
      {
        name: "Indulge Café",
        range: "$$$",
        discount: "50%",
        meals: 7,
        food: "Café",
        closing: "5:30 PM",
        position: {
          lat: -33.9066662,
          lng: 151.23162289999998
        },
      },
      {
        name: "Salad Bar",
        range: "$",
        discount: "60%",
        meals: 12,
        food: "Fast food/restaurant",
        closing: "5:00 PM",
        position: {
          lat: -33.8766662,
          lng: 151.2026228999998
        },
      },
      {
        name: "Joe's Sandwiches",
        range: "$",
        discount: "50%",
        meals: 8,
        food: "Fast food/restaurant",
        closing: "5:30 PM",
        position: {
          lat: -33.8566662,
          lng: 151.2316227999998
        },
      }
    ],
  }),
  created() {
    this.desktopViewActive();
  },
  mounted() {
    const self = this;

      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(function(position) {

          self.myPos.lat= position.coords.latitude;
          self.myPos.lng = position.coords.longitude;

          console.log(self.myPos);
          
        })
      } else {
        // Browser doesn't support Geolocation
        //console.log('geolocation not supported')
      }
  },
  methods: {
    /* mapClicked(e) {
      console.log(e.latLng)
    } */

    markerClicked(e) {
      console.log(e)
    },

    toggleInfoWindow: function(marker, idx) {
      this.infoWindowPos = marker.position;
      this.infoContent.name = marker.name;
      this.infoContent.range = marker.range;
      this.infoContent.discount = marker.discount;
      this.infoContent.meals = marker.meals;
      this.infoContent.food = marker.food;
      this.infoContent.closing = marker.closing;
      //check if its the same marker that was selected if yes toggle
      if (this.currentMidx == idx) {
        this.infoWinOpen = !this.infoWinOpen;
      }
      //if different marker set infowindow to open and reset current marker index
      else {
        this.infoWinOpen = true;
        this.currentMidx = idx;
      }
    },

    desktopViewActive: function () {
      var screenWidth = window.innerWidth;

      if (screenWidth >= 780) {
        this.desktop = true;
      }
      else {
        this.desktop = false;
      }
    },
  },
  computed: {
    /* filteredList() {
      return this.postList.filter(post => {
        return post.title.toLowerCase().includes(this.search.toLowerCase())
      })
    } */
  },
  components: {
    //GmapMap,
    //GmapMarker,
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

.c-map {
  &__container {
    width: 100vw;
    height: calc(100vh - 80px);
    position: relative;
    display: flex;
    flex-direction: column;
  }

  &__search {
    position: absolute;
    top: 20px;
    z-index: 10;
    align-self: center;

    input {
      width: inherit;
      min-width: 80vw;
      height: 50px;
      border: none;
      border-radius: 20px;
      border-style: none;
      -webkit-appearance: none;
      padding: 0 15px;
      font-size: 16px;
    }
  }

  &__footer {
    width: 100%;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    background-color: #ffffff;
    font-size: 16px;

    &-item {
      padding: 20px;

      &.active {
        border-bottom: 5px solid #000000;
      }

      &::before {
        content:'';
        display: block;
        width: 25px;
        height: 25px;
        background-repeat: no-repeat;
        background-size: contain;
        background-image: url('../assets/img/profile.png');
        margin: 0 auto;
        margin-bottom: 10px;
      }

      &:first-of-type::before {
        background-image: url('../assets/img/marker.png');
      }

      &:last-of-type::before {
        background-image: url('../assets/img/payment-method.png');
      }
    }
  }

  @media (min-width: 780px) {
    &__container {
      width: 60vw;
      height:60vh;
    }

    &__search {
      align-self: flex-start;
      padding-left: 20px;

      input{
        min-width: 400px;
      }
    } 

    &__info {
      width: 100%;
      max-width: 464px;
      height: 60%;
      max-height: 323px;
      background-image: url('../assets/img/rest.jpg');
      flex-grow: 1;
      background-size: cover;
      background-position: 100% 0;
      margin-left: auto;

    }
  }
}
</style>
