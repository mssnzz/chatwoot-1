<template>
  <woot-button
    variant="smooth"
    class="label--add"
    icon="add"
    size="tiny"
    @click="addLabel" style="background-color: #f5f5f5!important; color: #000!important; font-family: 'bipermedium'!important; font-size: 14px!important;"
  >
    {{ $t('CONTACT_PANEL.LABELS.CONVERSATION.ADD_BUTTON') }}
  </woot-button>
</template>

<script>
export default {
  methods: {
    addLabel() {
      this.$emit('add');
    },
  },
};
</script>

<style lang="scss" scoped>
.label--add {
  margin-bottom: var(--space-micro);
  margin-right: var(--space-micro);
}
</style>
