<template>
  <button
    @click="onClick()"
    :style="{ backgroundColor: color, color: fontColor }"
    class="btn"
  >
    {{ text }}
  </button>
</template>

<script setup>
// props
defineProps({
  text: String,
  color: String,
  fontColor: String,
});

// emits
const emit = defineEmits(['btn-click']);

// methods
const onClick = (e) => {
  emit('btn-click');
};
</script>
