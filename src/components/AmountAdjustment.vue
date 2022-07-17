<template>
    <input
      :class="{hidden}"
      type="button"
      :value='value'
      @click="setAmount(adjustTo)"
    >
</template>

<script>
import ModelInput from '@/components/ModelInput.vue';
import { MIN_AMOUNT, MAX_AMOUNT } from '@/constants';

export default {
  name: 'AmountAdjustment',
  extends: ModelInput,
  props: {
    value: {
      type: String,
      default: '',
    },
    adjustTo: {
      type: [Number, String],
      default: 0,
    },
    isChip: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      MIN_AMOUNT,
      MAX_AMOUNT,
    };
  },
  computed: {
    hidden() {
      console.log(this.adjustTo);
      return this.isChip && this.adjustTo >= MAX_AMOUNT;
    },
  },
  methods: {
    setAmount(value) {
      if (value < MIN_AMOUNT) {
        this.model = MIN_AMOUNT;
      } else if (value > MAX_AMOUNT) {
        this.model = MAX_AMOUNT;
      } else {
        this.model = Math.ceil(value);
      }
    },
  },
};
</script>

<style scoped>
.hidden {
  visibility: hidden;
}
</style>
