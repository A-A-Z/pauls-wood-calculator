<template>
  <div id="app">

    <div class="logo-preview">

      <div class="label-width">{{ (millimetres(logoSize, logoMetric) ) / 10 }} CM</div>
      <div class="label-height">{{ (millimetres(logoSize, logoMetric) ) / 10 }} CM</div>
      <div class="label-surface">{{ Math.round(surface) / 100 }} CM<sup>2</sup></div>

      <div class="logo"></div>

    </div>

    <div class="settings">
      <p>
        <SizeField
          label="Logo Size"
          :fieldSize="logoSize"
          :fieldMetric="logoMetric"
          :metrics="metrics"
          :onFieldSizeChange="onLogoSizeChange"
          :onFieldMetricChange="onLogoMetricChange"
        />
      </p>

      <p>
        <SizeField
          label="Block Size"
          :fieldSize="blockWidth"
          :fieldMetric="blockMetric"
          :metrics="metrics"
          :onFieldSizeChange="onBlockSizeChange"
          :onFieldMetricChange="onBlockMetricChange"
        />
      </p>


    </div>

    <div class="output">
      output: ({{ surface }})
    </div>

  </div>
</template>

<script>
import SizeField from './components/SizeField'

const data ={
  logoSize: 1,
  logoMetric: 1,
  blockWidth: 1,
  blockMetric: 1,
  metrics: [
    { long: 'millimetres', short: 'mm', value: 1 },
    { long: 'centimetres', short: 'cm', value: (1 * 10) },
    { long: 'metres', short: 'm', value: (1 * 10 * 100) }
  ],
  pi: 3.14159265359
}

export default {
  name: 'app',

  data: () => {
    return data
  },

  components: { SizeField },

  beforeMount () {
    // console.log(this.millimetres(this.logoSize, 1))
  },

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

    onLogoSizeChange (e) {
      const newVal = parseInt(e.target.value)
      this.logoSize = (newVal) ?  newVal : 1
    },

    onLogoMetricChange (e) {
      const newVal = parseInt(e.target.value)
      this.logoMetric = (newVal) ?  newVal : 0
    },

    onBlockSizeChange (e) {
      const newVal = parseInt(e.target.value)
      this.blockSize = (newVal) ?  newVal : 1
    },

    onBlockChange (e) {
      const newVal = parseInt(e.target.value)
      this.blockMetric = (newVal) ?  newVal : 0
    }
  },

  computed: {
    surface () {
      return this.getSurface(this.millimetres(this.logoSize, this.logoMetric))
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

body {

}

.logo-preview {
  background-color: $colour-logo-bg;
  color: $colour-logo;
  padding: 50px;

  .logo {
    background-color: $colour-logo;
    border-radius: 50% 50% 0 50%;
    height: $logo-size;
    width: $logo-size;
  }
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
