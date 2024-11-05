<template>
  <div v-if="_isVisible" :class="_styles.formGroup" class="mowi-master-item-search">
    <label :class="_styles.label">Modificación del {{ _label }}</label>
    <div :class="_styles.container">
      <Datepicker
        v-model="variable"
        :format="_formatComp"
        :mode="_mode"
        :input-class="'input-class'"
        @input="returnData"
        :minimum-view="'month'"
        :maximum-view="'year'"
      />
    </div>
  </div>
</template>

<script>
import moment from 'moment';
import Datepicker from 'vuejs-datepicker';

export default {
  name: 'dateComponent',
  components: {
    Datepicker,
  },
  props: {
    label: { type: String, default: 'Fecha' },
    dateRange: { type: Object, default: () => ({ start: '', end: '' }) },
    styles: { type: Object, default: () => ({}) },
    mode: { type: String, default: 'range' },
    isVisible: { type: Boolean, default: true },
    isRequired: { type: Boolean, default: false },
    format: { type: String, default: 'DD/MM/YYYY' },
  },
  data() {
    return {
      _label: this.label,
      _dateRange: this.dateRange,
      _styles: {
        formGroup: 'form-group col-md-6 col-sm-12 col-xs-12',
        label: 'control-label col-md-4 col-sm-4 col-xs-12',
        container: 'col-md-8 col-sm-8 col-xs-12',
        ...this.styles,
      },
      _mode: this.mode,
      _formatComp: this.format,
      _isVisible: this.isVisible,
      variable: this.mode === 'range' ? { start: '', end: '' } : '',
    };
  },
  watch: {
    isVisible(newValue) {
      this._isVisible = newValue;
    },
    dateRange(newValue) {
      this._dateRange = newValue;
      this.variable = this.mode === 'range' ? newValue : newValue.start; // Update variable based on mode
    },
  },
  methods: {
    returnData() {
      console.log('return data fecha', this.variable);
      if (this._mode === 'single') {
        const selectedDate = moment(this.variable).valueOf();
        this.$emit('input', selectedDate);
      } else if (this._mode === 'range') {
        const selectedRange = {
          start: moment(this.variable.start).valueOf(),
          end: moment(this.variable.end).valueOf(),
        };
        this.$emit('input', selectedRange);
      }
    },
    clearValue() {
      this.variable = this.mode === 'range' ? { start: '', end: '' } : '';
      this.$emit('input', undefined);
    },
  },
};
</script>
