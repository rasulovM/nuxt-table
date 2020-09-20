<template>
  <form @submit.prevent="addNewTagline(title)" class="popup">
    <h1 class="popup__title">Введите название</h1>
    <input type="text" v-model="title" class="popup__input" ref="popupInput" />
    <v-btn class="popup__button" type="submit" color="primary" dark
      >Сохранить</v-btn
    >
  </form>
</template>

<script>
export default {
  props: {
    addNewTagline: Function,
  },
  data() {
    return {
      title: "",
    };
  },
  mounted() {
    this.$refs.popupInput.focus();
  },
};
</script>

<style lang="scss" scoped>
.popup {
  position: fixed;
  height: fit-content;
  background: #ffffff;
  width: 400px;
  min-height: 300px;
  border-radius: 20px;
  padding: 30px 15px;
  left: 0;
  top: 0;
  bottom: 0;
  right: 0;
  margin: auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  z-index: 999;
  &__title {
    font-size: 20px;
  }
  &__input {
    margin: 30px 0;
    border: 1px solid grey;
    border-radius: 10px;
    height: 50px;
    outline: none;
    width: 100%;
    padding: 5px 0 5px 10px;
  }
  &__button {
    margin-top: auto;
  }
}
</style>
