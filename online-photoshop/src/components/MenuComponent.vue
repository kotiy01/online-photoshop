<template>
    <div class="menu">
        <button class="menu__btn" @click="handleImageUploadClick">Загрузить изображение</button>
        <p class="menu__text">или</p>
        <input class="menu__input" type="text" disabled placeholder="Вставьте ссылку на изображение" />
    </div>
</template>

<script>
export default {
  methods: {
    handleImageUploadClick() {
      const input = document.createElement('input');
      input.type = 'file';
      input.accept = 'image/*';
      input.addEventListener('change', this.$parent.$refs.upload.handleImageUpload);
      input.click();
    },
  },
};
</script>

<style lang="scss">
.menu {
    width: 100%;
    height: 40px;
    background-color: #CDCDCD;
    display: flex;
    justify-content: flex-start;
    align-items: center;

    &__btn {
      background-color: #fff;
      margin-left: 20px;
      border: 1px solid black;
      border-radius: 2px;

      &:hover {
        opacity: 0.6;
      }
    }

    &__text {
      margin-left: 20px;
    }

    &__input {
      width: 210px;
      border: 1px solid black;
      border-radius: 2px;
      margin-left: 20px;
    }
}
</style>