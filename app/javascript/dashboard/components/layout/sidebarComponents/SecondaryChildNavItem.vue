<template>
  <router-link
    v-slot="{ href, isActive, navigate }"
    :to="to"
    custom
    active-class="active"
  >
    <li :class="{ active: isActive }">
      <a
        :href="href"
        class="button clear menu-item text-truncate"
        :class="{ 'is-active': isActive, 'text-truncate': shouldTruncate }"
        @click="navigate"
      >
        <span v-if="icon" class="badge--icon">
          <fluent-icon class="inbox-icon" :icon="icon" size="14" />
        </span>
        <span
          v-if="labelColor"
          class="badge--label"
          :style="{ backgroundColor: labelColor }"
        />
        <span
          :title="menuTitle"
          class="menu-label button__content"
          :class="{ 'text-truncate': shouldTruncate }"
        >
          {{ label }}
          <span v-if="showChildCount" class="count-view">
            {{ childItemCount }}
          </span>
        </span>
        <span v-if="count" class="badge" :class="{ secondary: !isActive }">
          {{ count }}
        </span>
        <span v-if="warningIcon" class="badge--icon">
          <fluent-icon
            v-tooltip.top-end="$t('SIDEBAR.FACEBOOK_REAUTHORIZE')"
            class="inbox-icon"
            :icon="warningIcon"
            size="12"
          />
        </span>
      </a>
    </li>
  </router-link>
</template>
<script>
export default {
  props: {
    to: {
      type: String,
      default: '',
    },
    label: {
      type: String,
      default: '',
    },
    labelColor: {
      type: String,
      default: '',
    },
    shouldTruncate: {
      type: Boolean,
      default: false,
    },
    icon: {
      type: String,
      default: '',
    },
    warningIcon: {
      type: String,
      default: '',
    },
    count: {
      type: String,
      default: '',
    },
    showChildCount: {
      type: Boolean,
      default: false,
    },
    childItemCount: {
      type: Number,
      default: 0,
    },
  },
  computed: {
    showIcon() {
      return { 'text-truncate': this.shouldTruncate };
    },
    menuTitle() {
      return this.shouldTruncate ? this.label : '';
    },
  },
};
</script>
<style lang="scss" scoped>
$badge-size: var(--space-normal);
$label-badge-size: var(--space-slab);

@font-face {
  font-family: 'sfregular';
  src: url('../../../assets/font/sfpro-regular.ttf');
}
@font-face {
  font-family: 'sfmedium';
  src: url('../../../assets/font/sfpro-medium.ttf');
}
@font-face {
  font-family: 'sfproblack';
  src: url('../../../assets/font/sfpro-black.ttf');
}
.button {
  margin: var(--space-small) 0;
}

.menu-item {
  display: inline-flex;
  font-family: 'bipermedium';
  width: 100%;
  height: var(--space-medium);
  padding: var(--space-smaller) var(--space-smaller);
  margin: var(--space-smaller) 0;
  text-align: left;
  line-height: 1.2;
  font-size: 15px!important;
  letter-spacing: 0.4px!important;
  color: #2D2E2E;

  &:hover {
    background: #FFF3E6;
    color: #FF4F00;
  }

  &:focus {
    border-color: none;
  }

  &.is-active {
    background: #FFF3E6;
    color: #FF4F00;
  }
  &.is-active .count-view {
    background: var(--w-75);
    color: var(--w-500);
  }
}

.menu-label {
  flex-grow: 1;
}

.inbox-icon {
  font-size: var(--font-size-nano);
}

.badge--label,
.badge--icon {
  display: inline-flex;
  border-radius: var(--border-radius-small);
  margin-right: var(--space-smaller);
}

.badge--icon {
  align-items: center;
  height: $badge-size;
  justify-content: center;
  min-width: $badge-size;
}

.badge--label {
  height: $label-badge-size;
  min-width: $label-badge-size;
  margin-left: var(--space-smaller);
  border: 1px solid var(--color-border-light);
}

.badge.secondary {
  min-width: unset;
  background: var(--s-75);
  color: var(--s-600);
  font-weight: var(--font-weight-bold);
}

.count-view {
  background: var(--s-50);
  border-radius: var(--border-radius-normal);
  color: var(--s-600);
  font-size: var(--font-size-micro);
  font-weight: var(--font-weight-bold);
  margin-left: var(--space-smaller);
  padding: var(--space-zero) var(--space-smaller);
  line-height: var(--font-size-small);
}
</style>
