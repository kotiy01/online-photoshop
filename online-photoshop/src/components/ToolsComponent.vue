<template>
    <aside class="tools">
        <button class="tools__btn" @click="togglePipette"><img class="tools__icon" src="../assets/icons/pipette.png"></button>
    </aside>
</template>

<script>
export default {
  methods: {
    togglePipette() {
      this.$parent.$refs.upload.togglePipette();
    },
  },
};
</script>

<style lang="scss">
.tools {
    position: absolute;
    height: 592px;
    padding: 4px;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: flex-start;
    border-radius: 2px;
    background-color: #CDCDCD;
    margin-left: 20px;
    margin-top: 80px;

    &__btn {
        width: 32px;
        height: 32px;
        border: 1px solid black;
        border-radius: 2px;

        &:focus {
            background-color: #bcbcbc;
        }

        &:hover {
          opacity: 0.6;
        }
    }

    &__icon {
        width: 100%;
        height: 100%;
    }
}
</style>