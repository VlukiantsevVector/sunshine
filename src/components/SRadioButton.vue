<template>
  <label :class="$class(modifiers)">
    <input
      type="radio"
      :name="name"
      :checked="checked"
      :disabled="inactive"
      @change="onChange"
      @focus="hasFocus = true"
      @blur="hasFocus = false"
    >

    <span :class="$class('box')">
      <transition name="fade">
        <svg
          v-if="checked"
          :class="$class('selected')"
          width="16"
          height="16"
        >
          <path
            fill="#1C1C1C"
            d="M8,11 C6.34314575,11 5,9.65685425 5,8 C5,6.34314575
              6.34314575,5 8,5 C9.65685425,5 11,6.34314575 11,8 C11,9.65685425
              9.65685425,11 8,11 Z"
          />
        </svg>
      </transition>
    </span>
    <slot />
  </label>
</template>

<script>
import Vue from 'vue';
import mixins from 'vue-typed-mixins';

export default Vue.extend({
  name: 'SRadioButton',

  model: {
    prop: 'choice',
    event: 'input',
  },

  props: {
    value: {
      type: String,
      required: true,
    },

    choice: {
      type: String,
      default: undefined,
    },

    name: {
      type: String,
      default: '',
    },

    inactive: {
      type: Boolean,
      default: false,
    },
  },

  data() {
    return {
      checked: this.choice === this.value,
      hasFocus: false,
    };
  },

  watch: {
    choice(val) {
      this.checked = val === this.value;
    },
  },

  computed: {
    modifiers() {
      return {
        focus: this.hasFocus,
        inactive: this.inactive,
      };
    },
  },

  methods: {
    onChange() {
      this.checked = true;
      this.$emit('input', this.value);
    },
  },
});
</script>
