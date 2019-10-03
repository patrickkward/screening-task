<template>
  <div class="hello">
    <h1>{{latestValue}}</h1>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from "vue-property-decorator"; 
import { Observable } from "rxjs";


@Component export default class HelloWorld extends Vue { 
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
</style>

