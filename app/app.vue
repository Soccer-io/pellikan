<script setup lang="ts">
const route = useRoute()
const { init: initTheme } = useTheme()
const { init: initUser } = useUserData()
const { restore, sync } = useTabs()

useHead({
  title: 'Pellikan',
  link: [{ rel: 'icon', type: 'image/svg+xml', href: '/favicon.svg' }, { rel: 'icon', type: 'image/png', href: '/favicon.png' }],
  script: [{ innerHTML: THEME_BOOT, tagPosition: 'head' }],
})

onMounted(() => {
  initTheme()
  initUser()
  restore(route.fullPath === '/' ? DEFAULT_PATH : route.fullPath)
})

// The route drives the active tab. "/" only redirects, so it never becomes a tab.
watch(() => route.fullPath, (p) => { if (p !== '/') sync(p) })
</script>

<template>
  <div class="flex h-full pt-[env(safe-area-inset-top)] pl-[env(safe-area-inset-left)] pr-[env(safe-area-inset-right)]">
    <a
      href="#main"
      class="sr-only focus:not-sr-only focus:fixed focus:left-4 focus:top-4 focus:z-50 focus:bg-surface focus:p-3 focus:text-fg"
    >
      Skip to main content
    </a>
    <AppSidebar />
    <div class="flex min-w-0 flex-1 flex-col">
      <TopBar />
      <TabStrip />
      <main id="main" tabindex="-1" class="min-h-0 flex-1 border-t border-line">
        <NuxtPage />
      </main>
      <BottomNav />
    </div>
    <CommandPalette />
  </div>
</template>
