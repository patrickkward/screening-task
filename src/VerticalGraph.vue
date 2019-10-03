<template>
  <div id="graph">
    <h2>Bar Chart Example</h2>
    <!-- These are the custom components we'll create -->
    <!-- Values for `my-box` are percentages of the width of the canvas. -->
    <!-- Each bar will take up an equal space of the canvas. -->
    <my-canvas style="width: 100%; height: 600px;">
      <my-box class="graph"
        v-for="obj, index of chartValues"
        :x1="obj.x"
        :x2="obj.x + 1"
        :y1="obj.y"
        :y2="obj.y + 1"
      >
      </my-box>
    </my-canvas>
  </div>
</template>

<script>
import MyCanvas from './MyCanvas.vue';
import MyBox from './MyBox.vue';
import { Component, Prop, Vue, Watch } from "vue-property-decorator"; 
import { Observable } from "rxjs";

@Component({
  components: {
    MyCanvas,
    MyBox
  },
})

@Component export default class VerticalGraph extends Vue {
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
      }); 
    } 
  } 
  
}

var v = new Vue({
    data: {
      xVal:0,
      yVal:0,
      chartValues: [
        {x: 24, y: 24},
        {x: 50, y: 7},
        {x: 50, y: 60}
      ]
    },

  methods:  {
    addNewPoint: function () {
      this.chartValues.push({
        x: this.xVal,
        y: this.yVal
      })
      this.xVal = 0,
      this.yVal = 0
    }
  }
})

</script>

<style>
html, body {
  margin: 0;
  padding: 0;
}

#graph{
  position: relative;
  height: 100%;
  width: 20%;
}
</style>