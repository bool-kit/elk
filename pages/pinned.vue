<script setup lang="ts">
definePageMeta({
  middleware: 'auth',
})

const { t } = useI18n()

useHead({
  title: () => t('account.pinned'),
})
</script>

<template>
  <MainContent>
    <template #title>
      <NuxtLink to="/public/pinned" timeline-title-style flex items-center gap-2 @click="$scrollToTop">
        <div i-ri:pushpin-line />
        <span>{{ t('account.pinned') }}</span>
      </NuxtLink>
    </template>

    <TimelinePinned v-if="isHydrated && currentUser" />
  </MainContent>
</template>
