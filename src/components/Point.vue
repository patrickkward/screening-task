<script>

//https://alligator.io/vuejs/vue-html5-canvas/
function convertPercentWidthToPixels(percent, ctx){
  return Math.floor((ctx.canvas.width / 100) * percent);
}

function convertPercentHeightToPixels(percent, ctx){
  return  Math.floor((ctx.canvas.height / 100) * percent);
}

export default {
  // Gets us the provider property from the parent <my-canvas> component.
  inject: ['provider'],

  props: {
    x1: {
      type: Number,
      default: 0
    },
    y1: {
      type: Number,
      default: 0
    },
    x2: {
      type: Number,
      default: 0
    },
    y2: {
      type: Number,
      default: 0
    }
  },

  data () {
    return {

      //cached point
      cachedPoint: {
        x: null,
        y: null,
        width: null,
        height: null
      },

      newPoint:{
        x: null,
        y: null,
        width: null,
        height: null
      }
    }
  },

  computed: {
    calculatePoint () {
      const ctx = this.provider.context

      // Turn start / end percentages into x, y, width, height in pixels.
      const calculated = {
        x: convertPercentWidthToPixels(this.x1, ctx),
        y: convertPercentHeightToPixels(this.y1, ctx),
        width: convertPercentWidthToPixels(this.x2 - this.x1, ctx),
        height: convertPercentHeightToPixels(this.y2 - this.y1, ctx)
      }

      // Yes yes, side-effects. This lets us cache the box dimensions of the previous render.
      // before we re-calculate calculatePoint the next render.
      this.cachedPoint = calculated
      return calculated
    }
  },

  render () {
    // Since the parent canvas has to mount first, it's *possible* that the context may not be
    // injected by the time this render function runs the first time.
    if(!this.provider.context) return;
    const ctx = this.provider.context;

    // Keep a reference to the box used in the previous render call.
    const cachedPoint = this.cachedPoint
    // Calculate the new box. (Computed properties update on-demand.)
    const newBox = this.calculatePoint
    this.x = newBox.x;

    //clears the old point using the cache and creates the new point
    ctx.beginPath();
    ctx.clearRect(cachedPoint.x, cachedPoint.y, cachedPoint.width, cachedPoint.height);
    ctx.rect(newBox.x, newBox.y, newBox.width, newBox.height);
    ctx.fillStyle = this.color;
    ctx.fill();
  }
}
</script>