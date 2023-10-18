<template>
  <button @click="this.$emit('onClick()')" class="btn btn-success">{{ text }}</button>
</template>
<script>

export default {
  name: 'AddButton',
  props: {
    text: String,
  },
};
</script>
<style scoped>
button {
  margin-left: 170vh;
  margin-top: 20px;
  margin-bottom: 40px;
  color: white;
}
</style>
