<template lang="html">
  <div class="api-key-input">
    <label for="api-key">Harmonix API key</label>
    <input name="api-key" type="text" v-model="modelProxy">
    <small v-if="value">saved in localStorage for your convenience</small>
  </div>
</template>

<script>
export default {
  props: {
    value: String
  },
  computed: {
    modelProxy: {
      get () {
        return this.value
      },
      set (value) {
        this.$emit('input', value)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.api-key-input {
  margin: 1em;
  display: flex;
  flex-direction: column;
  align-items: center;

  label {
    font-weight: bold;
  }

  input {
    width: 260px;
    text-align: center;
    margin-top: 0.5em;
    padding: 0.25em;
  }

  small {
    color: lightgray;
  }
}
</style>
