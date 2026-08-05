<template>
  <div class="elegant-menu">
    <button class="elegant-menu__trigger" type="button" @click="isOpen = !isOpen">
      <IconMenu2 :size="16" />
      <span>{{ $t('common.menu') }}</span>
    </button>

    <transition name="menu-popover">
      <div v-if="isOpen" class="elegant-menu__popover">
        <div class="elegant-menu__popover-head">
          <span>{{ $t('common.menu') }}</span>
          <button type="button" class="elegant-icon-button" @click="isOpen = false">
            <IconX :size="16" />
          </button>
        </div>
        <div class="elegant-menu__grid">
          <router-link
            v-for="item in navItems"
            :key="item.name"
            :to="item.path"
            class="elegant-menu__item"
            :class="{ active: isActive(item) }"
            @click="isOpen = false"
          >
            <component :is="item.icon" :size="18" stroke-width="1.8" />
            <span>{{ $t(`menu.${item.i18nKey}`) }}</span>
            <IconChevronRight :size="14" class="elegant-menu__arrow" />
          </router-link>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import { computed, ref } from 'vue';
import { useRoute } from 'vue-router';
import { NAVIGATION_CONFIG } from '@/utils/baseConfig';
import IconDashboard from '@/components/icons/IconDashboard.vue';
import IconShop from '@/components/icons/IconShop.vue';
import IconInvite from '@/components/icons/IconInvite.vue';
import IconFileText from '@/components/icons/IconFileText.vue';
import IconWallet from '@/components/icons/IconWallet.vue';
import IconUser from '@/components/icons/IconUser.vue';
import IconMore from '@/components/icons/IconMore.vue';
import { IconChartBar, IconChevronRight, IconHeadset, IconMenu2, IconServer, IconX } from '@tabler/icons-vue';

export default {
  name: 'SlideTabsNav',
  components: {
    IconChartBar,
    IconChevronRight,
    IconHeadset,
    IconMenu2,
    IconServer,
    IconX,
    IconDashboard,
    IconShop,
    IconInvite,
    IconFileText,
    IconWallet,
    IconUser,
    IconMore
  },
  setup() {
    const route = useRoute();
    const isOpen = ref(false);

    const navItems = computed(() => {
      const map = {
        docs: { name: 'Docs', path: '/docs', icon: IconFileText, i18nKey: 'docs' },
        invite: { name: 'Invite', path: '/invite', icon: IconInvite, i18nKey: 'invite' },
        tickets: { name: 'Tickets', path: '/tickets', icon: IconHeadset, i18nKey: 'tickets' },
        nodes: { name: 'Nodes', path: '/nodes', icon: IconServer, i18nKey: 'nodes' },
        orders: { name: 'Orders', path: '/orders', icon: IconShop, i18nKey: 'orders' },
        traffic: { name: 'TrafficLog', path: '/trafficlog', icon: IconChartBar, i18nKey: 'traffic' },
        wallet: { name: 'Deposit', path: '/wallet/deposit', icon: IconWallet, i18nKey: 'wallet' },
        profile: { name: 'Profile', path: '/profile', icon: IconUser, i18nKey: 'profile' }
      };

      const items = [
        { name: 'Dashboard', path: '/dashboard', icon: IconDashboard, i18nKey: 'dashboard' },
        { name: 'Shop', path: '/shop', icon: IconShop, i18nKey: 'shop' }
      ];
      const third = map[NAVIGATION_CONFIG?.thirdNavItem || 'invite'];
      const fourth = map[NAVIGATION_CONFIG?.fourthNavItem || 'docs'];
      if (third) items.push(third);
      if (fourth && fourth.name !== third?.name) items.push(fourth);
      items.push({ name: 'More', path: '/more', icon: IconMore, i18nKey: 'more' });
      return items;
    });

    const isActive = (item) => route.name === item.name || route.meta?.activeNav === item.name;

    return { isOpen, navItems, isActive };
  }
};
</script>
