<script setup lang="ts">
// const props = withDefaults(defineProps<{}>(), {});

// На потом
// TODO: добавить тени сверху при прокрутке body__content вниз
// TODO: добавить тени снизу при прокрутке body__content вверх
</script>

<template>
  <div class="tn-dialog-body">
    <div class="tn-dialog-body__header" v-if="$slots.header">
      <slot name="header"></slot>
    </div>
    <div class="tn-dialog-body__content scrollbox">
      <slot></slot>
    </div>
  </div>
</template>

<style lang="scss">
.tn-dialog-body {
  position: relative;
  display: flex;
  flex-direction: column;
  height: 100%;
  overflow: hidden;
}

.tn-dialog-body__header {
  position: relative;
  width: 100%;
  padding: 8px 24px;

  border-bottom: var(--dialog-body-header-border-bottom);
  background-color: var(--dialog-body-header-bg);
  z-index: 1;
}

.tn-dialog-body__content {
  flex: 1;
  overflow-y: auto;
  padding: 24px;
  padding-right: 16px;

  scrollbar-gutter: stable;
}
</style>
