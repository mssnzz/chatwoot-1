<template>
  <transition-group
    name="wizard-items"
    tag="div"
    class="wizard-box"
    :class="classObject"
  >
    <div
      v-for="item in items"
      :key="item.route"
      class="item" style="background-color: #fff!important;"
      :class="{ active: isActive(item), over: isOver(item) }"
    >
      <div class="flex-align-center">
        <h3 class="text-truncate" style="font-family: 'bipermedium'!important;">
          {{ item.title }}
        </h3>
        <span v-if="isOver(item)" class="completed">
          <fluent-icon icon="checkmark" />
        </span>
      </div>
      <span class="step" style="font-family: 'biperegular'!important;">
        {{ items.indexOf(item) + 1 }}
      </span>
      <p style="font-family: 'biperegular'!important;">{{ item.body }}</p>
    </div>
  </transition-group>
</template>

<script>
/* eslint no-console: 0 */
import globalConfigMixin from 'shared/mixins/globalConfigMixin';

export default {
  mixins: [globalConfigMixin],
  props: {
    isFullwidth: Boolean,
    items: {
      type: Array,
      default: () => [],
    },
  },
  computed: {
    classObject() {
      return 'full-width';
    },
    activeIndex() {
      return this.items.findIndex(i => i.route === this.$route.name);
    },
  },
  methods: {
    isActive(item) {
      return this.items.indexOf(item) === this.activeIndex;
    },
    isOver(item) {
      return this.items.indexOf(item) < this.activeIndex;
    },
  },
};
</script>
