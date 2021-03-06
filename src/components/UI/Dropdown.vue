<template>
  <div class="dropdown">
    <label v-if="label" class="dropdown__label"> {{ label }} </label>
    <div
      class="dropdown__toggle"
      :class="{ 'dropdown__toggle--toggled': showMenu }"
      @click="toggleDropdown()"
    >
      <span>{{ getLabel(selected) }}</span>
      <i :class="{ open: showMenu }" class="material-icons">
        keyboard_arrow_down
      </i>
    </div>
    <div
      :style="{ height: showMenu ? height + 'px' : '0px' }"
      :class="{ 'dropdown__options--toggled': showMenu }"
      class="dropdown__options"
    >
      <div
        v-for="option in options"
        :key="option.value"
        class="dropdown__option"
        :title="option.text"
        @click="selectItem(option)"
      >
        <span>{{ option.text }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  inheritAttrs: false,
  props: {
    value: { type: [String, Number, Object], default: "" },
    options: {
      type: [Array, Object],
      default() {
        return [];
      },
    },
    label: { type: [String, null], default: null },
    placeholder: {
      type: String,
      default() {
        return this.$t("app.dropdown.placeholder" /* Select... */);
      },
    },
  },
  data() {
    return {
      selected:
        this.value === "" ? this.placeholder : this.getLabel(this.value),
      showMenu: false,
      height: this.options.length * 30,
    };
  },
  watch: {
    value() {
      this.selected =
        this.value === "" ? this.placeholder : this.getLabel(this.value);
    },
    options() {
      this.height = this.options.length * 30;
    },
  },
  methods: {
    toggleDropdown() {
      this.showMenu = !this.showMenu;
    },
    selectItem(option) {
      this.showMenu = false;
      this.selected = option.text;
      this.$emit("input", option.value);
      this.$emit("change");
    },
    getLabel(value) {
      const index = this.options.findIndex((option) => option.value === value);

      if (index === -1) return value;
      return this.options[index].text;
    },
  },
};
</script>

<style scoped lang="scss">
$button-color-bg: #101010;
$border-color-separate: #0d0d0d;
$highlight-color: #1a1a1a;
$highlight-color-text: #ffffff;
$text-color: #e6e6e6;
$border-color: #2c2c2c;
$border-color-expand: #2c2c2c;

$max-width: 200px;
$min-width: 140px;
.dropdown {
  position: relative;
  z-index: 999;
  font-size: 14px;
  font-weight: 500;
  color: $text-color;
  margin: 10px 4px 10px 0;
  &__label {
    color: #eee;
    font-size: 15px;
  }
  &__toggle {
    position: relative;
    width: 100%;
    min-width: $min-width;
    max-width: $max-width;

    text-overflow: ellipsis;
    white-space: nowrap;
    overflow: hidden;
    height: 30px;
    background: $button-color-bg;
    padding: 5px 30px 5px 10px;
    margin-top: 5px;
    //margin-left: 5px;
    margin-left: -1px;
    text-align: left;
    border-radius: 4px;
    cursor: pointer;
    border: 1px solid $border-color;
    transition: border-color 0.3s ease-in-out,
      border-radius 0.3s cubic-bezier(1, -0.21, 1, -1.65);

    &--toggled {
      border-radius: 4px 4px 0 0;
      border-color: $border-color-expand;
      border-bottom-color: transparent;
      transition: border-radius 0.3s cubic-bezier(0, 1.95, 0, 1.93);
    }
    &:hover {
      background: $highlight-color;
      color: $highlight-color-text;
    }
    i {
      position: absolute;
      top: 3px;
      right: 4px;
      display: inline-block;
      transition: transform 0.3s ease-in-out;
      cursor: pointer;

      &.open {
        transform: rotate(180deg);
      }
    }
    span {
      cursor: pointer;
    }
  }
  &__options {
    width: 100%;
    cursor: pointer;
    position: absolute;
    border-radius: 0 0 4px 4px;
    max-width: $max-width;
    background: $button-color-bg;
    transition: height 0.3s ease-in-out, border-color 0.2s ease-in-out;
    overflow: hidden;
    border: 1px solid transparent;
    margin-left: 5px;
    opacity: 0;
    &--toggled {
      opacity: 1;
      border-color: $border-color-expand;
      border-top-color: $border-color-separate;
    }
  }
  &__option {
    cursor: pointer;
    width: 100%;
    height: 30px;
    padding: 5px 25px 5px 5px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;

    &:last-child {
      border-radius: 0 0 4px 4px;
    }
    position: relative;
    &:hover {
      background: $highlight-color;
    }

    span {
      cursor: pointer;
      padding: 0 5px;
    }
  }
}
</style>
