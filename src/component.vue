<template lang="html">
  <input type="tel"
         :value="formattedValue"
         @change="change"
         v-money="{precision, decimal, thousands, prefix, suffix, emptyValue}"
         class="v-money" />
</template>

<script>
import money from './directive'
import defaults from './options'
import {format, unformat} from './utils'

export default {
  name: 'Money',
  props: {
    value: {
      required: true,
      // type: [Number, String],
      validator: prop => typeof prop === 'number' || typeof prop === 'string' || prop === null || prop === undefined,
      default: null
    },
    masked: {
      type: Boolean,
      default: false
    },
    precision: {
      type: Number,
      default: () => defaults.precision
    },
    decimal: {
      type: String,
      default: () => defaults.decimal
    },
    thousands: {
      type: String,
      default: () => defaults.thousands
    },
    prefix: {
      type: String,
      default: () => defaults.prefix
    },
    suffix: {
      type: String,
      default: () => defaults.suffix
    },
    emptyValue: {
      default: () => null
    }
  },

  directives: {money},

  data () {
    return {
      formattedValue: ''
    }
  },

  watch: {
    value: {
      immediate: true,
      handler (newValue, oldValue) {
        var formatted = format(newValue, this.$props)
        if (formatted !== this.formattedValue) {
          this.formattedValue = formatted
        }
      }
    }
  },

  methods: {
    change (evt) {
      this.$emit('input', this.masked ? evt.target.value : unformat(evt.target.value, this.$props))
    }
  }
}
</script>
