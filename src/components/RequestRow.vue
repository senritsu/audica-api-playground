<template lang="html">
  <div class="request-row" :class="{first: step === 1}">
    <div class="step" @click="$emit('click')">
      {{ step }}
      <div v-if="hint" class="hint-ring"></div>
    </div>
    <div class="request-button" @click="$emit('click')">
      <button>
        <slot />
      </button>
    </div>
    <div class="settings">
      <slot name="settings" />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    step: Number,
    running: Boolean,
    hint: Boolean
  }
}
</script>

<style lang="scss" scoped>
.request-row {
  margin: 1em;
  display: flex;
  align-items: flex-start;

  .step, .request-button {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 60px;

    cursor: pointer;

    button {
      cursor: pointer;
    }
  }

  .step {
    position: relative;
    border-radius: 50%;
    border: 7px solid darkorange;

    transition: border-color 0.25s;

    width: 60px;
    font-size: 2em;

    .hint-ring {
      position: absolute;
      width: 100%;
      height: 100%;

      border: 3px solid lightgray;
      border-radius: 50%;
      animation: 2s infinite hint;
    }
  }

  .request-button {
    margin-left: -5px;

    button {
      width: 120px;
      text-align: left;

      padding: 0.5em;
      background-color: darkorange;
      color: whitesmoke;
      border: none;
      border-radius: 3px;

      transition: background-color 0.25s;

      &:hover {
        background-color: orange;
      }
    }
  }

  &.first {
    .step {
      border-color: deepskyblue;
    }
    .request-button button {
      background-color: deepskyblue;
    }
  }

  &:hover {
    .step {
      border-color: orange;
    }
    button {
      background-color: orange
    }

    &.first {
      .step {
        border-color: skyblue;
      }
      button {
        background-color: skyblue
      }
    }
  }

  .settings {
    margin-left: 2em;
    display: flex;
  }
}

@keyframes hint {
  0% {
    transform: scale(2.5);
    opacity: 0.5;
  }
  100% {
    transform: scale(1);
    opacity: 1;
  }
}
</style>
