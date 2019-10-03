<template>
  <div id="graph">
    <h2>{{latestValue}}</h2>
    <my-canvas>
      <my-box
        v-for="obj, index of coordinates"
        :x1="obj.x"
        :x2="obj.x + 1"
        :y1="obj.y"
        :y2="obj.y + 1"
      >
      </my-box>
    </my-canvas>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from "vue-property-decorator"; 
import { Observable } from "rxjs";
import MyCanvas from './Canvas.vue';
import MyBox from './Point.vue';

@Component({
  components: {
    MyCanvas,
    MyBox
  },
})

 export default class VerticalGraph extends Vue { 
     data(){
        return{
            coordinates: [
                {x: 24, y: 24},
                {x: 50, y: 7},
                {x: 50, y: 60}
            ]  
        }
    }

  @Prop({ default: null }) 
  private source?: Observable<number>; 
  latestValue: number = 5; 
  @Watch("source", { immediate: true }) 
  onSourceChange() { 
    console.log("onSourceChange"); 
    if (this.source != undefined) { 
      const self = this; 
      this.source.subscribe(x => { 
        self.latestValue = x; 
        this.coordinates.push({x: self.latestValue + (Math.random() * 100), y:Math.random() * 100});
      }); 
    } 
  }
} 

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

#graph{
  position: relative;
  height: 100%;
  width: 20%;
}
</style>

