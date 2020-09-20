<template>
  <div class="cell">
    <input
      type="text"
      class="cell__input pt-2 pb-2"
      :class="{ 'cell__input--active': edit }"
      ref="tableInput"
      v-model="value"
      @blur="clearEdit"
      :placeholder="placeholder"
      @focus="edit = true"
      v-on:keyup.enter="confirm"
    />
    <v-icon
      color="white"
      v-if="edit"
      small
      class="ml-2 cell__icon"
      @click="confirm()"
      >mdi-check</v-icon
    >
  </div>
</template>

<script>
export default {
  name: "Cell",
  props: {
    title: [String, Number],
    indxCol: Number,
    indxRow: Number,
  },
  data() {
    return {
      edit: false,
      value: this.$props.title,
      placeholder: "Empty",
    };
  },
  watch: {
    title(value) {
      this.value = value;
    },
  },
  methods: {
    confirm() {
      this.clearEdit();
      this.$refs.tableInput.blur();
      this.$emit(
        "updateCell",
        this.value,
        this.$props.title,
        this.$props.indxRow,
        this.$props.indxCol
      );
    },
    clearEdit() {
      this.edit = false;
    },
  },
};
</script>

<style lang="scss" scoped>
.cell {
  width: 100%;
  position: relative;
  &__title {
    display: block;
  }
  &__icon {
    position: absolute;
    right: 10px;
    top: 0;
    bottom: 0;
    margin: auto 0;
    width: 16px;
    height: 16px;
    z-index: 99999;
    background: #4763f0;
    border-radius: 5px;
  }
  &__input {
    border: none;
    outline: none;
    width: 100%;
    padding: 0 26px;
    border-bottom: 1px solid transparent;
    &:focus {
      border-color: #4a4a4a;
    }
  }
}
</style>
