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
        <label>Logo width</label>
        <input type="number" v-model.number="logoSize">
        <select v-model.number="logoMetric">
          <option v-for="(metric, index) in metrics" :value="index" :key="metric.value">{{ metric.short }}</option>
        </select>
      </p>

      <p>
        <label>Block width</label>
        <input type="number" v-model.number="blockWidth">
        <select v-model.number="blockMetric">
          <option v-for="(metric, index) in metrics" :value="index" :key="metric.value">{{ metric.short }}</option>
        </select>
      </p>
    </div>

    <div class="output">
      output: ({{ surface }})
    </div>

  </div>
</template>

<script>

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
