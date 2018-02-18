<template>
  <div class="field field-size">
    <div><label>{{ label }}</label></div>
    <div>
      <input
        type="number"
        min="1"
        v-on:input="onSizeChange"
        :value="fieldSize"
      >
      <select v-on:change="onMetricChange">
        <option
          v-for="(metric, index) in metrics"
          :value="index"
          :key="metric.value"
          :selected="(fieldMetric === index)"
        >{{ metric.short }}</option>
      </select>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SizeField',

  props: {
    id: {
      type: String,
      required: true
    },
    label: {
      type: String,
      default: 'Unlabeled Field'
    },
    fieldSize: {
      type: Number,
      default: 1
    },
    fieldMetric: {
      type: Number,
      default: 0
    },
    metrics: {
      type: Array,
      required: true
    },
    onFieldSizeChange: {
      type: Function,
      default: (e) => { console.log('Size Changed', e.target.value) }
    },
    onFieldMetricChange: {
      type: Function,
      default: (e) => { console.log('Matric Changed', e.target.value) }
    }
  },

  methods: {
    onSizeChange(e) {
      const newVal = parseInt(e.target.value)
      this.onFieldSizeChange(this.id, (newVal >= 0) ?  newVal : 0)
    },

    onMetricChange(e) {
      const newVal = parseInt(e.target.value)
      this.onFieldMetricChange(this.id, (newVal >= 0) ?  newVal : 1)
    }
  }
}
</script>
