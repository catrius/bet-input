<script>
export default {
  name: 'ModelInput',
  props: {
    modelValue: {
      type: null,
      default: null,
    },
  },
  emits: ['update:modelValue'],
  computed: {
    model: {
      get() {
        return this.modelValue;
      },
      set(value) {
        this.$emit('update:modelValue', value);
      },
    },
  },
};
</script>
