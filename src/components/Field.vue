<script>
export default {
  props: {
    column: {
      type: Number,
      required: true,
    },
    row: {
      type: Number,
      required: true,
    },
    disabled: {
      type: Boolean,
      default: () => false,
    },
    value: {
      type: Number,
    },
  },
};
</script>

<template>
  <div class="game__field" @click="$emit('change', column, row)">
    {{ value }}
  </div>
</template>

