<template>
  <div class="field field-size">
    <label>{{ label }}</label>
    <input
      type="number"
      min="1"
      v-on:input="onFieldSizeChange"
      :value="fieldSize"
    >
    <select v-on:change="onFieldMetricChange">
      <option
        v-for="(metric, index) in metrics"
        :value="index"
        :key="metric.value"
        :selected="(fieldMetric === index)"
      >{{ metric.short }}</option>
    </select>

  </div>
</template>

<script>
export default {
  name: 'SizeField',

  props: {
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
  }
}
</script>
