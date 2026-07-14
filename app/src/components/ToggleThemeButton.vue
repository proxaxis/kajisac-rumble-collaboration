<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import IconSun from '@/components/icons/IconSun.vue';
import IconMoon from '@/components/icons/IconMoon.vue';

const isDarkTheme = ref(false);
const theme = ref('light');
const mediaQuery = ref(null);

function toggleTheme() {
  const newTheme = isDarkTheme.value ? 'light' : 'dark';
  applyTheme(newTheme);
}

function getSystemTheme() {
  if (window.matchMedia('(prefers-color-scheme: dark)').matches) {
    return 'dark';
  }
  return 'light';
}

function applyTheme(t) {
  isDarkTheme.value = t === 'dark';
  document.documentElement.setAttribute('data-theme', t);
  theme.value = t;
}

function handleThemeChange(e) {
  applyTheme(e.matches ? 'dark' : 'light');
}

onMounted(() => {
  const systemTheme = getSystemTheme();
  console.log('System theme is:', systemTheme);

  applyTheme(systemTheme);

  mediaQuery.value = window.matchMedia('(prefers-color-scheme: dark)');

  mediaQuery.value.addEventListener('change', handleThemeChange);
});

onUnmounted(() => {
  mediaQuery.value.removeEventListener('change', handleThemeChange);
});
</script>

<template>
  <button class="toggle-theme-button" @click="toggleTheme">
    <IconSun size="1.5rem" v-if="isDarkTheme" />
    <IconMoon size="1.5rem" v-else />
  </button>
</template>
