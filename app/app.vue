<script setup lang="ts">
useHead({
  meta: [{ name: 'viewport', content: 'width=device-width, initial-scale=1' }],
  link: [{ rel: 'icon', href: '/favicon.ico' }],
  htmlAttrs: { lang: 'zh-CN' }
})

const title = '栖木商店 · 经营中心'
const description = '面向小型电商经营者的日常经营仪表盘'
useSeoMeta({ title, description, ogTitle: title, ogDescription: description })

const sidebarOpen = ref(false)
const navigation = [
  { label: '经营概览', icon: 'i-lucide-layout-dashboard', to: '/', active: true },
  { label: '订单管理', icon: 'i-lucide-receipt-text', badge: '8' },
  { label: '商品库存', icon: 'i-lucide-package', badge: '3' },
  { label: '客户管理', icon: 'i-lucide-users' },
  { label: '营销活动', icon: 'i-lucide-megaphone' },
  { label: '数据分析', icon: 'i-lucide-chart-no-axes-combined' }
]
</script>

<template>
  <UApp>
    <div class="app-shell">
      <aside :class="['sidebar', { 'sidebar--open': sidebarOpen }]">
        <div class="brand-row">
          <div class="brand-mark">栖</div>
          <div><p class="brand-name">栖木商店</p><p class="brand-plan">基础版</p></div>
          <UButton class="sidebar-close" icon="i-lucide-x" color="neutral" variant="ghost" aria-label="关闭导航" @click="sidebarOpen = false" />
        </div>

        <nav class="main-nav" aria-label="主要导航">
          <p class="nav-label">工作台</p>
          <NuxtLink
            v-for="item in navigation"
            :key="item.label"
            :to="item.to || '#'"
            :class="['nav-item', { 'nav-item--active': item.active }]"
            @click="sidebarOpen = false"
          >
            <UIcon :name="item.icon" class="size-5" />
            <span>{{ item.label }}</span>
            <span v-if="item.badge" class="nav-badge">{{ item.badge }}</span>
          </NuxtLink>
        </nav>

        <div class="sidebar-bottom">
          <a href="#" class="nav-item"><UIcon name="i-lucide-circle-help" class="size-5" /><span>帮助与反馈</span></a>
          <a href="#" class="nav-item"><UIcon name="i-lucide-settings" class="size-5" /><span>店铺设置</span></a>
          <div class="account-row">
            <UAvatar alt="林小满" size="sm" />
            <div class="account-copy"><p>林小满</p><span>店铺管理员</span></div>
            <UButton icon="i-lucide-chevrons-up-down" color="neutral" variant="ghost" size="xs" aria-label="切换账号" />
          </div>
        </div>
      </aside>

      <div v-if="sidebarOpen" class="sidebar-backdrop" @click="sidebarOpen = false" />
      <div class="workspace">
        <header class="topbar">
          <UButton class="mobile-menu" icon="i-lucide-menu" color="neutral" variant="ghost" aria-label="打开导航" @click="sidebarOpen = true" />
          <div class="store-status"><span class="status-dot" />店铺营业中</div>
          <div class="topbar-actions">
            <UButton icon="i-lucide-search" color="neutral" variant="ghost" aria-label="搜索" />
            <UChip color="error" inset><UButton icon="i-lucide-bell" color="neutral" variant="ghost" aria-label="通知" /></UChip>
            <UColorModeButton />
            <UButton icon="i-lucide-plus" label="发布商品" color="primary" />
          </div>
        </header>
        <main class="page-content"><NuxtPage /></main>
      </div>
    </div>
  </UApp>
</template>
