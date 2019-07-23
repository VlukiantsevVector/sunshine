<template>
  <s-text-field
    v-model="internalValue"
    :custom-is-valid-key="(keyCode)=>isValidKeyNumber(keyCode)"
    :label="label"
  />
</template>

<script>
import Vue from 'vue';
import STextField from './STextField.vue';

export default Vue.extend({
  name: 'SNumberField',

  $_veeValidate: {
    value() {
      return this.internalValue;
    },
  },

  components: {
    STextField,
  },

  props: {
    label: {
      type: String,
      default: '',
    },
    allowFloat: {
      type: Boolean,
      default: false,
    },
    value: {
      type: Number,
      default: 0,
    },
  },

  data() {
    return {
      internalValue: this.value.toString(),
    };
  },

  watch: {
    value(val) {
      this.internalValue = val.toString();
    },

    internalValue(val) {
      if (parseInt(val, 10) !== this.value) {
        this.$emit('input', val === '' ? 0 : val);
      }
    },
  },

  computed: {
    isEmpty() {
      return !Number.isInteger(this.value) && !this.internalValue && !this.placeholder;
    },
  },

  methods: {
    isValidKeyNumber(keyCode) {
      if (this.allowFloat) {
        // to get decimal separator we need to formatToParts number with fraction
        // and get [1] element, which states for decimal separator part
        let keyCodeDecimalSeparator = Intl.NumberFormat(this.$root.$i18n.locale)
          .formatToParts(1.1)[1].value.charCodeAt(0);
        return (
          keyCode === 45 || //-
          keyCode === keyCodeDecimalSeparator ||
          (keyCode >= 48 && keyCode <= 57) // numbers
        );
      } else return (keyCode >= 48 && keyCode <= 57) || keyCode === 43;
    },
  },
});
</script>
