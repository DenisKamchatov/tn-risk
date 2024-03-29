<script setup lang="ts">
import {
  PropType,
  defineProps,
  computed,
  useSlots,
  defineEmits,
  getCurrentInstance,
} from "vue";

import { useRouter } from "vue-router";

import TNIcon from "@/components/uikit/elements/icons/tn-icon.vue";
import TnLoader from "@/components/uikit/elements/loader/tn-loader.vue";
// TODO: добавить лоадер из кита и вернуть состояние загрузки в кнопку
// import TnLoader from "@/components/uikit/elements/elements/loader/tn-loader.vue";

const props = withDefaults(
  defineProps<{
    size?: "md" | "lg",
    block?: boolean,
    primary?: boolean,
    secondary?: boolean,
    isIcon?: boolean,
    disabled?: boolean,
    icon?: string,
    iconRight?: string,
    href?: string,
    norouter?: boolean,
    target?: string,
    width?: number | string,
    loading?: boolean,
  }>(),
  {
    size: "lg",
    primary: true,
  }
);

const router = useRouter();

const sizeOutput = computed<string | boolean>(() => {
  if (props.size) {
    return props.size;
  }
  return false;
});

const loaderSize = computed(() => {
  return props.size;
});

const emit = defineEmits(["click"]);
const slots = useSlots();

const hasContent = computed<boolean>(() => !!slots["default"]);
const hasIconSlot = computed<boolean>(() => !!slots["icon"]);
const hasRightIconSlot = computed<boolean>(() => !!slots["icon-right"]);
const hasIcon = computed<boolean>(() => hasIconSlot.value || !!props.icon);
const isOnlyIcon = computed<boolean>(() => !hasContent.value && hasIcon.value && !props.isIcon);
const hasRightIcon = computed<boolean>(
  () => hasRightIconSlot.value || !!props.iconRight
);

const buttonWidth = computed(() => {
  if (props.width && props.width.toString().includes("%")) {
    return props.width;
  }

  if (props.width && props.width.toString().includes("px")) {
    return props.width;
  }

  return props.width + "%";
});

function onClickButton(e: Event) {
  if (props.loading || props.disabled) {
    return false;
  }

  if (props.href) {
    if (props.norouter || props.target === "_blank") {
      if (props.target === "_blank") {
        window.open(props.href, "_blank");
      } else {
        window.location.href = props.href;
      }
    } else {
      router?.push(props.href);
    }
  }

  emit("click", e);
}
</script>

<template>
  <button
    class="tn-button"
    :disabled="disabled"
    :class="{
      'tn-button_medium': sizeOutput === 'md',
      'tn-button_large': sizeOutput === 'lg',
      'tn-button_primary': primary,
      'tn-button_secondary': secondary,
      'tn-button_only-icon': isOnlyIcon,
      'tn-button_block': block,
      'tn-button_disabled': disabled,
      'tn-button_is-icon': isIcon,
      'tn-button_loading': loading,
    }"
    :style="{ width: buttonWidth }"
    @click="onClickButton"
  >
    <span
      v-if="hasIcon"
      class="tn-button__icon"
    >
      <slot name="icon">
        <TNIcon :size="20" :name="icon" />
      </slot>
    </span>

    <span v-if="hasContent && !isIcon" class="tn-button__text">
      <slot></slot>
    </span>

    <span v-if="hasRightIcon && !isIcon" class="tn-button__icon">
      <slot name="icon-right">
        <TNIcon :size="20" :name="iconRight" />
      </slot>
    </span>

    <span v-if="loading" class="tn-button__loader">
      <TnLoader :size="loaderSize" class="tn-button__loader-icon" />
    </span>
  </button>
</template>

<style lang="scss">
.tn-button {
  display: inline-flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
  position: relative;

  height: 48px;
  padding: 14px 24px;

  border-radius: 12px;
  font-size: 14px;
  font-weight: 600;
  line-height: 20px;

  border: none;
  background: none;
  user-select: none;

  outline: 1px solid transparent;
  outline-offset: 1px;
  transition: 300ms;

  &:disabled,
  &[disabled] {
    opacity: 0.5;
    cursor: initial;
  }

  &.tn-button_only-icon {
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 0;
    min-width: 48px;
    max-width: 48px;
    height: 48px;
  }

  .tn-badge {
    font-weight: 600;
  }
}
.tn-button__icon {
  display: flex;
  align-items: center;
  justify-content: center;

  position: relative;
  font-size: 20px;

  svg {
    pointer-events: none;
  }
}

.tn-button_medium {
  height: 40px;
  padding: 10px 24px;
  gap: 8px;

  border-radius: 8px;

  &.tn-button_only-icon {
    min-width: 40px;
    max-width: 40px;
    height: 40px;
  }
}

.tn-button_primary {
  color: var(--button-primary-color);
  background-color: var(--button-primary-bg);

  .tn-button__icon {
    color: var(--button-primary-color);
  }

  &:hover:enabled {
    background: var(--button-primary-bg-hover);

    transition: 300ms;
  }

  &:active:enabled {
    background: var(--button-primary-bg-active);

    transition: 300ms;
  }

  &:focus {
    animation: focus-animation-primary 500ms ease forwards;
    transition: 300ms;
  }

  .tn-badge {
    background-color: var(--button-primary-badge-bg);
    color: var(--button-primary-bg);
  }
}

.tn-button_secondary {
  color: var(--button-secondary-color);
  background-color: var(--button-secondary-bg);

  .tn-button__icon {
    color: var(--button-secondary-color);
  }

  &:hover:enabled {
    background: var(--button-secondary-bg-hover);

    transition: 300ms;
  }

  &:active:enabled {
    background: var(--button-secondary-bg-active);

    transition: 300ms;
  }

  &:focus {
    animation: focus-animation-secondary 500ms ease forwards;
    transition: 300ms;
  }

  .tn-badge {
    background-color: var(--button-secondary-badge-bg);
    color: var(--button-secondary-badge-color);
  }

  .tn-button__loader {
    .tn-loader__spinner {
      border-color: var(--button-secondary-color);
      border-top-color: transparent;
    }
  }
}

.tn-button_medium.tn-button_is-icon {
  width: 24px;
  height: 24px;

  span {
    font-size: 20px;
  }
}

.tn-button_large.tn-button_is-icon {
  width: 32px;
  height: 32px;

  span {
    font-size: 24px;
  }
}

.tn-button_is-icon {
  background: none;
  border-radius: 8px;
  min-width: 0;

  padding: 0;
  margin: 0;

  color: inherit;

  .tn-button__icon {
    color: inherit;
  }

  &:hover:enabled {
    background: var(--button-secondary-bg);
  }

  &:focus {
    background: var(--button-secondary-bg);
    animation: none;
  }
}

.tn-button_block {
  display: flex;
  width: 100%;
}

.tn-button_loading {
  pointer-events: none;

  .tn-button__icon {
    opacity: 0;
  }
  .tn-button__text {
    opacity: 0;
  }
}

.tn-button__loader {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 20px;
}

@keyframes focus-animation-secondary {
  from {
    outline: 1px solid #ffffff00;
    outline-offset: 1px;
  }
  to {
    outline: 1px solid #9ea5b5;
    outline-offset: 1px;
  }
}

@keyframes focus-animation-primary {
  from {
    outline: 1px solid #ffffff00;
    outline-offset: 1px;
  }
  to {
    outline: 1px solid #d91921;
    outline-offset: 1px;
  }
}
</style>
