<template>
  <div id="app" class="wood-calculator">

    <div class="logo-preview">

      <div class="label-width">
        {{ (millimetres(sizes.logoWidth.size, sizes.logoWidth.metric) ) / 10 }} x
        {{ (millimetres(sizes.logoWidth.size, sizes.logoWidth.metric) ) / 10 }} CM
      </div>
      <div class="label-surface">{{ Math.round(surface) / 100 }} CM<sup>2</sup></div>

      <div class="logo"></div>

    </div>

    <div class="settings">
      <SizeField
        id="logoWidth"
        label="Logo Size"
        :fieldSize="sizes.logoWidth.size"
        :fieldMetric="sizes.logoWidth.metric"
        :metrics="metrics"
        :onFieldSizeChange="updateSize"
        :onFieldMetricChange="updateMetric"
      />

      <SizeField
        id="blockWidth"
        label="Block Width"
        :fieldSize="sizes.blockWidth.size"
        :fieldMetric="sizes.blockWidth.metric"
        :metrics="metrics"
        :onFieldSizeChange="updateSize"
        :onFieldMetricChange="updateMetric"
      />

      <SizeField
        id="blockMin"
        label="Block Min Length"
        :fieldSize="sizes.blockMin.size"
        :fieldMetric="sizes.blockMin.metric"
        :metrics="metrics"
        :onFieldSizeChange="updateSize"
        :onFieldMetricChange="updateMetric"
      />

      <SizeField
        id="blockMax"
        label="Block Max Length"
        :fieldSize="sizes.blockMax.size"
        :fieldMetric="sizes.blockMax.metric"
        :metrics="metrics"
        :onFieldSizeChange="updateSize"
        :onFieldMetricChange="updateMetric"
      />

      <div class="field field-cost">
        <div><label>Wood cost (per meter)</label></div>
        <div>
          <input
            type="number"
            min="0.01"
            v-model="costPerMeter"
          >
        </div>
      </div>

    </div>

    <div class="output">
      <p>
        Number of blocks: {{ blockCount }}
      </p>

      <p>
        Total length of blocks: {{ blockLength / 1000 }} Meters
      </p>

      <p>
        Total cost: ${{ totalCost }}
      </p>
    </div>

  </div>
</template>

<script>
import SizeField from './components/SizeField'

const data ={
  sizes: {
    logoWidth: { size: 1, metric: 1 },
    blockWidth: { size: 1, metric: 1 },
    blockMin: { size: 1, metric: 1 },
    blockMax: { size: 1, metric: 1 }
  },
  metrics: [
    { long: 'millimetres', short: 'mm', value: 1 },
    { long: 'centimetres', short: 'cm', value: (1 * 10) },
    { long: 'metres', short: 'm', value: (1 * 10 * 100) }
  ],
  costPerMeter: 5
}

export default {
  name: 'app',

  data: () => {
    return data
  },

  components: { SizeField },

  methods: {
    millimetres(size, metric) {
      return size * this.metrics[metric].value
    },

    getSurface (width) {
      const radius = width / 2
      const circleSurface = radius * radius * Math.PI
      const squireSurface = width * width
      return (circleSurface * 0.75) + (squireSurface * 0.25)
    },

    updateSize (fieldId, value) {
      this.sizes[fieldId].size = value
    },

    updateMetric (fieldId, value) {
      this.sizes[fieldId].metric = value
    }
  },

  computed: {
    surface () {
      return this.getSurface(this.millimetres(this.sizes.logoWidth.size, this.sizes.logoWidth.metric))
    },

    blockCount () {
      const blockWidthMm = this.millimetres(this.sizes.blockWidth.size, this.sizes.blockWidth.metric)
      const blockArea = blockWidthMm * blockWidthMm
      return Math.ceil(this.surface / blockArea)
    },

    blockLength () {
      const lengthMinMm = this.millimetres(this.sizes.blockMin.size, this.sizes.blockMin.metric)
      const lengthMaxMm = this.millimetres(this.sizes.blockMax.size, this.sizes.blockMax.metric)
      const avgLength = (lengthMinMm + lengthMaxMm) / 2
      return avgLength * this.blockCount
    },

    totalCost () {
      const meters = Math.ceil(this.blockLength / 1000 )
      return meters * this.costPerMeter
    }
  }
}
</script>

<style lang="scss">

// colours
$colour-logo: #FFF;
$colour-logo-bg: #444;

// sizes
$logo-size: 200px;


.wood-calculator {
  display: flex;

  &> div {
    border: 1px solid #CCC;
    padding: 15px;
    width: 33%;
  }
}

.logo-preview {
  background-color: $colour-logo-bg;
  color: $colour-logo;

  .logo {
    background-color: $colour-logo;
    border-radius: 50% 50% 0 50%;
    height: $logo-size;
    margin: 20px;
    width: $logo-size;
  }
}

.field {
  margin-bottom: 15px;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
