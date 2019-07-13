<template>
  <div class="c-main" id="app">
    <div class="c-main__header">
      <h1 class="c-main__logo">
        <img src="@/assets/img/logo.png">
      </h1>
      <nav v-if="desktop">
        <a href="">About</a>
        <a href="">Contact</a>
        <a href="#download">Download</a>
        <a href="">Login</a>
      </nav>
    </div>
    <h2 class="c-main__subheader" v-if="desktop">Let's fight food waste<br>
    Find deals near you:</h2>
    <Map />
    
    <div id="download">
    
    </div>
  </div>
</template>

<script>
import Map from './components/Map.vue'

export default {
  name: 'app',
  data: ()=> ({
    desktop: false
  }),
  created() {
    this.desktopViewActive();
  },
  methods: {
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
  components: {
    Map
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Open+Sans:400,800&display=swap');
@import 'assets/styles/reset.scss';

.c-main {
  min-height: 100vh;
  height: 100%;
  font-family: 'Open Sans', sans-serif;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  font-size: 16px;

  &__header {
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
  }

  nav {
    font-size: 16px;
    width: 100%;
    display: flex;
    flex-direction: row;
    align-content: center;
    justify-content: flex-end;
    align-self: center;

    a {
      padding: 20px;
    }
  }

  &__logo {
    height: 80px;
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
  }

  &__subheader {
    font-size: 48px;
    font-weight: 900;
    text-align: center;
    padding: 40px 0;
  }

  @media (min-width: 780px) {

    &__logo {

    }
  }
}
</style>
