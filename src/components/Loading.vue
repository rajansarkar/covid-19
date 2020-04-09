<template>
  <div class="spinner-border spinner-border-sm" :class="colorName" role="status">
    <span class="sr-only">Loading...</span>
  </div>
</template>

<script>
export default {
  props: ["colorName"]
};
</script>

<style>
</style>