<template>
  <div class="my-canvas-wrapper">
    <canvas ref="graph-canvas"></canvas>
    <slot></slot>
  </div>
</template>

<script>

//https://alligator.io/vuejs/vue-html5-canvas/

export default {
  data() {
    return {
      // By creating the provider in the data property, it becomes reactive,
      // so child components will update when `context` changes.
      provider: {
        // This is the CanvasRenderingContext that children will draw to.
        context: null
      }
    }
  },

  // Allows any child component to `inject: ['provider']` and have access to it.
  provide () {
    return {
      provider: this.provider
    }
  },

  mounted () {
    //sends canvas to all children.
    this.provider.context = this.$refs['graph-canvas'].getContext('2d')

    //Resizes the canvas.
    this.$refs['graph-canvas'].width = 200
    this.$refs['graph-canvas'].height = 500
  }
}
</script>