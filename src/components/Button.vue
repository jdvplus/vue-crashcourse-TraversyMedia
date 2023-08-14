<template>
  <button
    @click="onClick()"
    :style="{ backgroundColor: color, color: fontColor }"
    class="btn"
  >
    {{ text }}
  </button>
</template>

<script>
export default {
  name: 'Button',
  props: {
    text: String,
    color: String,
    fontColor: String,
  },
  methods: {
    onClick() {
      // console.log('clicked');
      this.$emit('btn-click');
    },
  },
};
</script>
