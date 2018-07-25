<template>
  <div id="app">
    <Game/>
    <Footer/>
  </div>
</template>

<script>
import Game from './components/Game'
import Footer from './components/Footer'

export default {
  name: 'App',
  components: {
    Game,
    Footer
  }
}
</script>

<style>
/* google font */
@import url('https://fonts.googleapis.com/css?family=Indie+Flower|Questrial|Quicksand:400,700&subset=latin-ext');
@import url('http://code.ionicframework.com/ionicons/2.0.1/css/ionicons.min.css');
/* some reset codes */
ul,li{
  padding:0;
  margin:0;
  list-style-type:none;
}
*,
*::before,
*::after{
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
*:focus{
  outline:0;
}
html,body {
  height:100%;
}
body{
  font-family: 'Quicksand', sans-serif;
  font-size:1rem;
  padding:0;
  margin:0;
  background: #005C97;
  background: -webkit-linear-gradient(to left, #363795, #005C97);
  background: linear-gradient(to left, #363795, #005C97);
}
#app {
  position:relative;
  height:100%;
  font-weight:bold;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #000;
}
</style>
