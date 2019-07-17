<template>
  <s-base-input
    :inactive="inactive"
    :has-focus="hasFocus"
    :is-empty="isEmpty"
    :readonly="readonly"
    :error="error"
    :label="label"
    :current-length="currentLength"
    class="s-number-field"
  >
    <input
      v-model.number="internalValue"
      :format="hasFocus ? format : ''"
      :type="'number'"
      :disabled="inactive"
      :readonly="readonly"
      :placeholder="placeholder"
      :class="{ 's-input__input': true, 's-input__input--with-label': !!label }"
      @focus="onFocus"
      @blur="onBlur"
      @keypress="onKeyPress"
      v-bind="$attrs"
    >
  </s-base-input>
</template>

<script>
import Vue from 'vue';
import SBaseInput from './SBaseInput.vue';
import SFormatInput from './internal/SFormatInput.vue';

export default Vue.extend({
  name: 'SNumberField',

  inheritAttrs: false,

  $_veeValidate: {
    value() {
      return this.internalValue;
    },
  },

  components: {
    SBaseInput,
  },

  props: {
    label: {
      type: String,
      default: '',
    },
    error: {
      type: String,
      default: undefined,
    },
    value: {
      type: Number,
      default: 0,
    },
    format: {
      type: String,
      default: null,
    },
    inactive: {
      type: Boolean,
      default: false,
    },
    readonly: {
      type: Boolean,
      default: false,
    },
    placeholder: {
      type: String,
      default: undefined,
    },
    allowFloat: {
      type: Boolean,
      default: false,
    },
  },

  data() {
    return {
      internalValue: this.value,
      hasFocus: false,
      formattedValue: this.value,
    };
  },

  watch: {
    value(val) {
      this.internalValue = val;
    },

    internalValue(val) {
      if (val !== this.value) {
        this.$emit('input', val === '' ? 0 : val);
      }
    },
  },

  computed: {
    isEmpty() {
      return !Number.isInteger(this.value) && !this.internalValue && !this.placeholder;
    },

    currentLength() {
      let val = this.internalValue;
      return val instanceof String ? val.length : val.toString().length;
    },
  },

  methods: {
    onBlur() {
      this.hasFocus = false;
      this.$emit('blur');
    },

    onFocus() {
      this.hasFocus = true;
      this.$emit('focus');
    },

    onKeyPress(event) {
      if (!this.isValidKey(event.keyCode)) {
        event.preventDefault();
      }
    },

    isValidKey(keyCode) {
      if (this.allowFloat) {
        return (
          keyCode === 43 ||
          keyCode === 45 ||
          keyCode === 46 ||
          (keyCode >= 48 && keyCode <= 57)
        );
      } else return keyCode >= 48 && keyCode <= 57;
    },
  },
});
</script>
