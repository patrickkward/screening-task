<template>
  <div id="app">
    <VerticalGraph :source=source class="temp"/>
    <VerticalGraph :source=source class="temp"/>
    <VerticalGraph :source=source class="temp"/>
    <VerticalGraph :source=source class="temp"/>
    <VerticalGraph :source=source class="temp"/>
    <!--<HelloWorld :source=source class="temp"></HelloWorld>
    <HelloWorld :source=source class="temp"></HelloWorld>
    <HelloWorld :source=source class="temp"></HelloWorld>
    <HelloWorld :source=source class="temp"></HelloWorld> 
    <HelloWorld :source=source class="temp"></HelloWorld>//-->
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import { Observable, interval, of, from } from 'rxjs'; 
import HelloWorld from './components/HelloWorld.vue';
import VerticalGraph from './components//VerticalGraph.vue';
import { map, delay } from 'rxjs/operators';

function populateRandomArray(source_amount:any){
  var temp = new Array();
  var i;
  for(i = 0; i < source_amount; i++){
   temp.push(Math.floor(Math.random() * 101 ));
  }

  return temp;
}

var hello = HelloWorld;
var hello2 = HelloWorld;

var x_coordinates = populateRandomArray(100);

@Component({
  components: {
    HelloWorld,
    VerticalGraph
  },
})

export default class App extends Vue {
  source: Observable<number> = from(x_coordinates).pipe(map(x => Math.sin(x)));
  source0: Observable<number> = from([2,4,6,8,10]).pipe(delay(5000));

  created(){
    this.source = interval(100).pipe(map(x => Math.sin(x)));
    }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.temp {
  width: 20%;
  background-color: grey;
  float: left;
}
</style>
