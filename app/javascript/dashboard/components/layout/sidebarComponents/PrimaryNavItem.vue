<template>
  <router-link v-slot="{ href, isActive, navigate }" :to="to" custom>
    <a
      v-tooltip.right="$t(`${name}`)"
      :href="href"
      class="button clear button--only-icon menu-item"
      :class="{ 'is-active': isActive || isChildMenuActive }"
      :rel="openInNewPage ? 'noopener noreferrer nofollow' : undefined"
      :target="openInNewPage ? '_blank' : undefined"
      @click="navigate"
    >
      <fluent-icon :icon="icon" />
      <span class="show-for-sr">{{ name }}</span>
      <span v-if="count" class="badge warning">{{ count }}</span>
    </a>
    
  </router-link>
  <a
      v-tooltip.right="Marketplace"
      href="https://biper.do/marketplace"
      class="button clear button--only-icon menu-item"
    >
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-6 h-6">
      <path fill-rule="evenodd" d="M3 6a3 3 0 013-3h2.25a3 3 0 013 3v2.25a3 3 0 01-3 3H6a3 3 0 01-3-3V6zm9.75 0a3 3 0 013-3H18a3 3 0 013 3v2.25a3 3 0 01-3 3h-2.25a3 3 0 01-3-3V6zM3 15.75a3 3 0 013-3h2.25a3 3 0 013 3V18a3 3 0 01-3 3H6a3 3 0 01-3-3v-2.25zm9.75 0a3 3 0 013-3H18a3 3 0 013 3V18a3 3 0 01-3 3h-2.25a3 3 0 01-3-3v-2.25z" clip-rule="evenodd" />
    </svg>

      <span class="show-for-sr">Marketplace</span>
    </a>
</template>
<script>
export default {
  props: {
    to: {
      type: String,
      default: '',
    },
    name: {
      type: String,
      default: '',
    },
    icon: {
      type: String,
      default: '',
    },
    count: {
      type: String,
      default: '',
    },
    isChildMenuActive: {
      type: Boolean,
      default: false,
    },
    openInNewPage: {
      type: Boolean,
      default: false,
    },
  },
};
</script>
<style lang="scss" scoped>
.button {
  margin: var(--space-small) 0;
}

.menu-item {
  display: inline-flex;
  position: relative;
  border-radius: var(--border-radius-large);
  border: 1px solid transparent;
  color: #2D2E2E;

  &:hover {
    color: #fff;
    background-color: #FF4F00;
  }

  &:focus {
    color: #fff;
    background-color: #FF4F00;
  }

  &.is-active {
    color: #fff;
    background-color: #FF4F00;
  }
}

.icon {
  font-size: var(--font-size-default);
}

.badge {
  position: absolute;
  right: var(--space-minus-smaller);
  top: var(--space-minus-smaller);
}
</style>
