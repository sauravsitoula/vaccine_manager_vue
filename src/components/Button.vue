<template>
  <button @click="onClick()" style=" background: lightblue" class="btn">
    Add Vaccine
  </button>
</template>

<script>
export default {
  name: 'Button',
  props: {
  },
  methods: {
    onClick() {
      this.$emit('btn-click')
    },
  },
}
</script>
