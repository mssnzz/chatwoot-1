<template>
  <div
    v-if="globalConfig.brandName && !disableBranding"
    class="px-0 py-3 flex justify-center"
  >
    <a
      href="https://openchat.com.do/"
      rel="noreferrer noopener nofollow"
      target="_blank"
      class="branding--link justify-center"
    >
      <img
        class="branding--image"
        :alt="globalConfig.brandName"
        src="https://beautiful-curran.146-190-54-185.plesk.page/openchat/openchat-logo.svg"
      />
      <span>
         Con la tecnología de Biper
      </span>
    </a>
  </div>
  <div v-else class="p-3" />
</template>

<script>
import globalConfigMixin from 'shared/mixins/globalConfigMixin';

const {
  LOGO_THUMBNAIL: logoThumbnail,
  BRAND_NAME: brandName,
  WIDGET_BRAND_URL: widgetBrandURL,
} = window.globalConfig || {};

export default {
  mixins: [globalConfigMixin],
  props: {
    disableBranding: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      globalConfig: {
        brandName,
        logoThumbnail,
        widgetBrandURL,
      },
    };
  },
  computed: {
    brandRedirectURL() {
      try {
        const referrerHost = this.$store.getters['appConfig/getReferrerHost'];
        const baseURL = `${this.globalConfig.widgetBrandURL}?utm_source=${
          referrerHost ? 'widget_branding' : 'survey_branding'
        }`;
        if (referrerHost) {
          return `${baseURL}&utm_referrer=${referrerHost}`;
        }
        return baseURL;
      } catch (e) {
        // Suppressing the error as getter is not defined in some cases
      }
      return '';
    },
  },
};
</script>

<style scoped lang="scss">
@import '~widget/assets/scss/variables.scss';
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@500;600;700;800&display=swap');
.branding--image {
  margin-right: $space-smaller;
  max-width: $space-slab;
  max-height: $space-slab;
  font-family: 'Inter'!important
}

.branding--link {
  color: $color-light-gray;
  cursor: pointer;
  display: inline-flex;
  filter: grayscale(1);
  font-size: $font-size-small;
  opacity: 0.9;
  text-decoration: none;

  &:hover {
    filter: grayscale(0);
    opacity: 1;
    color: $color-gray;
  }
}
</style>
