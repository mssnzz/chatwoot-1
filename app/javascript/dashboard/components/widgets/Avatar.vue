<template>
  <div class="avatar-container" :style="style" aria-hidden="true">
    {{ userInitial }}
  </div>
</template>

<script>
export default {
  name: 'Avatar',
  props: {
    username: {
      type: String,
      default: '',
    },
    size: {
      type: Number,
      default: 40,
    },
  },
  computed: {
    style() {
      return {
        fontSize: `${Math.floor(this.size / 2.5)}px`,
      };
    },
    userInitial() {
      const parts = this.username.split(/[ -]/);
      let initials = parts.reduce((acc, curr) => acc + curr.charAt(0), '');

      if (initials.length > 2 && initials.search(/[A-Z]/) !== -1) {
        initials = initials.replace(/[a-z]+/g, '');
      }
      initials = initials.substring(0, 2).toUpperCase();

      return initials;
    },
  },
};
</script>

<style lang="scss" scoped>
.avatar-container {
  display: flex;
  line-height: 100%;
  font-weight: 800;
  align-items: center;
  justify-content: center;
  text-align: center;
  background: #B0C9FA;
  color: #000;
  cursor: default;
  font-size: 12px;
}
</style>
