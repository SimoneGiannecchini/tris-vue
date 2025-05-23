<script setup>
import '../assets/cella.css'

const props = defineProps({
  value: String,
  index: Number
})

const emit = defineEmits(['click'])

function handleClick() {
  emit('click', props.index)
}
</script>

<template>
  <div class="cella" @click="handleClick">
    {{ value }}
  </div>
</template>
