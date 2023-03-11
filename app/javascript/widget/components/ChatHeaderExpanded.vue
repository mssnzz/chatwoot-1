
<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@500;600;700;800&display=swap');

.inter{
font-family: 'Inter'!important;
}
</style>
<template>
  <header
    class="header-expanded py-6 px-5 relative box-border w-full"
    :class="$dm('bg-white', 'dark:bg-slate-900')"
  >
    <div
      class="flex items-start"
      :class="[avatarUrl ? 'justify-between' : 'justify-end']"
    >
      <img
        v-if="avatarUrl"
        class="h-12 rounded-full"
        :src="avatarUrl"
        alt="Avatar"
      />
      <header-actions :show-popout-button="showPopoutButton" />
    </div>
    <h2
      v-dompurify-html="introHeading"
      class="mt-5 text-3xl mb-3 font-normal" style="font-family: 'Inter'!important; font-weight: 800!important; font-size: 34px; margin-top: 15%"
      :class="$dm('text-slate-900', 'dark:text-slate-50')"
    />
    <h2
      v-dompurify-html="introBody"
      class="text-lg leading-normal inter" style="font-family: 'Inter'!important; font-weight: 500!important; font-size: 15px!important; padding-bottom: 7%!important; "
      :class="$dm('text-slate-700', 'dark:text-slate-200')"
    />
  </header>
</template>

<script>
import { mapGetters } from 'vuex';
import HeaderActions from './HeaderActions';
import darkModeMixin from 'widget/mixins/darkModeMixin.js';

export default {
  name: 'ChatHeaderExpanded',
  components: {
    HeaderActions,
  },
  mixins: [darkModeMixin],
  props: {
    avatarUrl: {
      type: String,
      default: '',
    },
    introHeading: {
      type: String,
      default: '',
    },
    introBody: {
      type: String,
      default: '',
    },
    showPopoutButton: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    ...mapGetters({
      widgetColor: 'appConfig/getWidgetColor',
    }),
  },
};
</script>
