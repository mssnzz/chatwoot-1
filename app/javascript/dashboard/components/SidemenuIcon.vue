<template>
  <button type="submit" class="button toggle-sidebar clear button--only-icon 26 secondary"  @click="onMenuItemClick" style="padding-left: 4%!important;">
    <fluent-icon icon="list" class="toggle-sidebar" size="26" />
  </button>

</template>

<script>
import { BUS_EVENTS } from 'shared/constants/busEvents';

export default {
  methods: {
    onMenuItemClick() {
      bus.$emit(BUS_EVENTS.TOGGLE_SIDEMENU);
    },
  },
};
</script>
<style scoped lang="scss">
.toggle-sidebar {
  margin-right: var(--space-small);
  margin-left: var(--space-minus-small);
}
</style>
