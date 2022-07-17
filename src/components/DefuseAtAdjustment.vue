<template>
    <input
      type="button"
      :value='value'
      @click="setAmount(adjustTo)"
    >
</template>

<script>
import ModelInput from '@/components/ModelInput.vue';
import { MIN_DEFUSE_AT, MAX_DEFUSE_AT } from '@/constants';

export default {
  name: 'DefuseAtAdjustment',
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
  },
  data() {
    return {
      MIN_DEFUSE_AT,
      MAX_DEFUSE_AT,
    };
  },
  methods: {
    setAmount(value) {
      if (value < MIN_DEFUSE_AT) {
        this.model = MIN_DEFUSE_AT;
      } else if (value > MAX_DEFUSE_AT) {
        this.model = MAX_DEFUSE_AT;
      } else {
        this.model = Number.parseFloat(value.toFixed(2));
      }
    },
  },
};
</script>
