<script setup lang="ts">
import TnIcon from "@/components/uikit/elements/icons/tn-icon.vue";

const { isBig } = withDefaults(
  defineProps<{
    isBig?: boolean;
  }>(),
  {
    isBig: false,
  }
);

interface IFooterItems {
  title: string;
  icon: string;
  href: string;
  to: string;
}

const items: IFooterItems[] = [
  {
    title: "Структура СВК",
    icon: "book",
    href: "",
    to: "",
  },
  {
    title: "Документы",
    icon: "files",
    href: "",
    to: "",
  },
  {
    title: "Роли и права",
    icon: "users",
    href: "",
    to: "",
  },
  {
    title: "Настройка процессов",
    icon: "settings",
    href: "",
    to: "",
  },
  {
    title: "Настройка чеклистов",
    icon: "list-check",
    href: "",
    to: "",
  },
];
</script>

<template>
  <div class="main-footer" :class="{ 'main-footer_big container': isBig }">
    <ul class="main-footer__left-bar">
      <li class="main-footer__item" v-for="item in items" :key="item.title">
        <TnIcon :name="item.icon" :size="24" />
        <p class="main-footer__item-text">{{ item.title }}</p>
      </li>
    </ul>
    <div class="main-footer__right-bar">
      <p class="main-footer__support-text">Техподдержка</p>
      <p class="main-footer__tn-risk">TN Risk — 2022</p>
    </div>
  </div>
</template>

<style lang="scss">
.main-footer {
  position: sticky;
  bottom: 0;
  left: 0;

  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 56px;

  width: 100%;
  padding: 40px 50px;

  background-color: #fff;
  border-top: 1px solid #e7e9ef;

  backdrop-filter: blur(5px);

  .main-footer__left-bar {
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    gap: 56px;
  }

  .main-footer__item {
    display: flex;
    align-items: center;

    font-size: 24px;

    cursor: pointer;
  }

  .main-footer__item-text {
    margin: 0 0 0 8px;

    font-size: 14px;
    font-weight: 600;
    line-height: 20px;
    white-space: nowrap;

    color: #2e384b;
  }

  .main-footer__right-bar {
    display: flex;
    align-items: center;
    gap: 40px;

    color: #2e384b;
    font-family: "Proxima Nova";
    font-size: 14px;
    font-weight: 600;
    line-height: 20px;
  }
}

.main-footer_big {
  background: none;
}

@media (max-width: 1400px) {
  .main-footer {
    gap: 32px;

    .main-footer__left-bar {
      gap: 32px;
    }
    .main-footer__right-bar {
      gap: 32px;
    }
  }
}
</style>
