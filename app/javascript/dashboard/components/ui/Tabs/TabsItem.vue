<template>
  <li
    :class="{
      'tabs-title': true,
      'is-active': active,
    }" style="font-family: 'bipermedium'; "
  >
    <a @click="onTabClick" style="font-size: 14px;">
      {{ name }}
      
      <span v-if="showBadge" class="badge" style="font-family: 'biperbold'; background: #FFF3E6; color: #FF4F00;">  
        {{ getItemCount }}
      </span>
       
    </a>
  </li>
</template>
<script>
export default {
  name: 'WootTabsItem',
  props: {
    index: {
      type: Number,
      default: 0,
    },
    name: {
      type: String,
      required: true,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    count: {
      type: Number,
      default: 0,
    },
    showBadge: {
      type: Boolean,
      default: true,
    },
  },

  computed: {
    active() {
      return this.index === this.$parent.index;
    },

    getItemCount() {
      return this.count;
    },
  },

  methods: {
    onTabClick(event) {
      event.preventDefault();
      if (!this.disabled) {
        this.$parent.$emit('change', this.index);
      }
    },
  },
};
</script>
