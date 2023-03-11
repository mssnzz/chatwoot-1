<template>
  <div class="contact--profile">
    <div class="contact--info">
      <thumbnail
        v-if="showAvatar"
        :src="contact.thumbnail"
        size="56px"
        :username="contact.name"
        :status="contact.availability_status"
      />

      <div class="contact--details">
        <div v-if="showAvatar" class="contact--name-wrap">
          <h3 class="sub-block-title contact--name" style="font-family: 'bipermedium'!important;">
            {{ contact.name }}
          </h3>
          <a
            :href="contactProfileLink"
            class="fs-default"
            target="_blank"
            rel="noopener nofollow noreferrer"
          >
            <woot-button
              size="tiny"
              icon="open"
              variant="clear"
              color-scheme="secondary"
            />
          </a>
        </div>
        <p v-if="additionalAttributes.description" class="contact--bio">
          {{ additionalAttributes.description }}
        </p>
        <social-icons :social-profiles="socialProfiles" />
        <div class="contact--metadata">
          <contact-info-row
            :href="contact.email ? `mailto:${contact.email}` : ''"
            :value="contact.email"
            icon="mail"
            emoji="✉️"
            :title="$t('CONTACT_PANEL.EMAIL_ADDRESS') " style="font-family: 'biperegular'!important;"
            show-copy
          />
          <contact-info-row
            :href="contact.phone_number ? `tel:${contact.phone_number}` : ''"
            :value="contact.phone_number"
            icon="call"
            emoji="📞"
            :title="$t('CONTACT_PANEL.PHONE_NUMBER')" style="font-family: 'biperegular'!important;"
          />
          <contact-info-row
            v-if="contact.identifier"
            :value="contact.identifier"
            icon="contact-identify"
            emoji="🪪"
            :title="$t('CONTACT_PANEL.IDENTIFIER')"
          />
          <contact-info-row
            :value="additionalAttributes.company_name"
            icon="building-bank"
            emoji="🏢"
            :title="$t('CONTACT_PANEL.COMPANY')" style="font-family: 'biperegular'!important;"
          />
          <contact-info-row
            v-if="location || additionalAttributes.location"
            :value="location || additionalAttributes.location"
            icon="map"
            emoji="🌍"
            :title="$t('CONTACT_PANEL.LOCATION')" style="font-family: 'biperegular'!important;"
          />
        </div>
      </div>
      <div class="contact-actions">
        <button type="submit" class="button clear button--only-icon secondary has-tooltip" 
        v-tooltip="$t('CONTACT_PANEL.NEW_MESSAGE')" style="color: #000!important; background-color: #f5f5f5!important; border-radius: 200px!important;"
          title="$t('CONTACT_PANEL.NEW_MESSAGE')" @click="toggleConversationModal">
          
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2.0" stroke="currentColor" class="w-6 h-6" style="height: 20px; width: 20px;">
            <path stroke-linecap="round" stroke-linejoin="round" d="M12 9v6m3-3H9m12 0a9 9 0 11-18 0 9 9 0 0118 0z" />
          </svg>

    
        </button>
        &nbsp;
        <button type="submit" class="button clear button--only-icon secondary has-tooltip" 
        v-tooltip="$t('EDIT_CONTACT.BUTTON_LABEL')" style="color: #000!important; background-color: #f5f5f5!important; border-radius: 200px!important;"
        title="$t('EDIT_CONTACT.BUTTON_LABEL')" @click="toggleEditModal">
          
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2.0" stroke="currentColor" class="w-6 h-6" style="width: 20px; height: 20px;">
          <path stroke-linecap="round" stroke-linejoin="round" d="M16.862 4.487l1.687-1.688a1.875 1.875 0 112.652 2.652L6.832 19.82a4.5 4.5 0 01-1.897 1.13l-2.685.8.8-2.685a4.5 4.5 0 011.13-1.897L16.863 4.487zm0 0L19.5 7.125" />
        </svg>


    
        </button>
        &nbsp;
        <button v-if="isAdmin" type="submit" class="button clear button--only-icon secondary has-tooltip" 
        v-tooltip="$t('CONTACT_PANEL.MERGE_CONTACT')" style="color: #000!important; background-color: #f5f5f5!important; border-radius: 200px!important;"
        title="$t('CONTACT_PANEL.MERGE_CONTACT')" :disabled="uiFlags.isMerging"
          @click="openMergeModal">
          
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2.0" stroke="currentColor" class="w-6 h-6" style="width: 20px; height: 20px;">
            <path stroke-linecap="round" stroke-linejoin="round" d="M7.5 21L3 16.5m0 0L7.5 12M3 16.5h13.5m0-13.5L21 7.5m0 0L16.5 12M21 7.5H7.5" />
          </svg> 
        </button>
        &nbsp;
        <button v-if="isAdmin" type="submit" class="button clear button--only-icon secondary has-tooltip" 
        v-tooltip="$t('DELETE_CONTACT.BUTTON_LABEL')" style="color: #E23740!important; background-color: #FFEBEE!important; border-radius: 200px!important;"
        title="$t('DELETE_CONTACT.BUTTON_LABEL')" :disabled="uiFlags.isDeleting"
          @click="toggleDeleteModal">
          
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2.0" stroke="currentColor" class="w-6 h-6" style="width: 20px; height: 20px;">
            <path stroke-linecap="round" stroke-linejoin="round" d="M14.74 9l-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 01-2.244 2.077H8.084a2.25 2.25 0 01-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 00-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 013.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 00-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 00-7.5 0" />
          </svg>




    
        </button>
        
      </div>
      <edit-contact
        v-if="showEditModal"
        :show="showEditModal"
        :contact="contact"
        @cancel="toggleEditModal"
      />
      <new-conversation
        v-if="contact.id"
        :show="showConversationModal"
        :contact="contact"
        @cancel="toggleConversationModal"
      />
      <contact-merge-modal
        v-if="showMergeModal"
        :primary-contact="contact"
        :show="showMergeModal"
        @close="toggleMergeModal"
      />
    </div>
    <woot-delete-modal
      v-if="showDeleteModal"
      :show.sync="showDeleteModal"
      :on-close="closeDelete"
      :on-confirm="confirmDeletion"
      :title="$t('DELETE_CONTACT.CONFIRM.TITLE')"
      :message="$t('DELETE_CONTACT.CONFIRM.MESSAGE')"
      :message-value="confirmDeleteMessage"
      :confirm-text="$t('DELETE_CONTACT.CONFIRM.YES')"
      :reject-text="$t('DELETE_CONTACT.CONFIRM.NO')"
    />
  </div>
</template>
<script>
import { mixin as clickaway } from 'vue-clickaway';

import ContactInfoRow from './ContactInfoRow';
import Thumbnail from 'dashboard/components/widgets/Thumbnail.vue';
import SocialIcons from './SocialIcons';

import EditContact from './EditContact';
import NewConversation from './NewConversation';
import ContactMergeModal from 'dashboard/modules/contact/ContactMergeModal';
import alertMixin from 'shared/mixins/alertMixin';
import adminMixin from '../../../../mixins/isAdmin';
import { mapGetters } from 'vuex';
import { getCountryFlag } from 'dashboard/helper/flag';

export default {
  components: {
    ContactInfoRow,
    EditContact,
    Thumbnail,
    SocialIcons,
    NewConversation,
    ContactMergeModal,
  },
  mixins: [alertMixin, adminMixin, clickaway],
  props: {
    contact: {
      type: Object,
      default: () => ({}),
    },
    channelType: {
      type: String,
      default: '',
    },
    showAvatar: {
      type: Boolean,
      default: true,
    },
  },
  data() {
    return {
      showEditModal: false,
      showConversationModal: false,
      showMergeModal: false,
      showDeleteModal: false,
    };
  },
  computed: {
    ...mapGetters({ uiFlags: 'contacts/getUIFlags' }),
    contactProfileLink() {
      return `/app/accounts/${this.$route.params.accountId}/contacts/${this.contact.id}`;
    },
    additionalAttributes() {
      return this.contact.additional_attributes || {};
    },
    location() {
      const {
        country = '',
        city = '',
        country_code: countryCode,
      } = this.additionalAttributes;
      const cityAndCountry = [city, country].filter(item => !!item).join(', ');

      if (!cityAndCountry) {
        return '';
      }
      return this.findCountryFlag(countryCode, cityAndCountry);
    },
    socialProfiles() {
      const {
        social_profiles: socialProfiles,
        screen_name: twitterScreenName,
      } = this.additionalAttributes;

      return { twitter: twitterScreenName, ...(socialProfiles || {}) };
    },
    // Delete Modal
    confirmDeleteMessage() {
      return ` ${this.contact.name}?`;
    },
  },
  methods: {
    toggleMergeModal() {
      this.showMergeModal = !this.showMergeModal;
    },
    toggleEditModal() {
      this.showEditModal = !this.showEditModal;
    },
    toggleConversationModal() {
      this.showConversationModal = !this.showConversationModal;
    },
    toggleDeleteModal() {
      this.showDeleteModal = !this.showDeleteModal;
    },
    confirmDeletion() {
      this.deleteContact(this.contact);
      this.closeDelete();
    },
    closeDelete() {
      this.showDeleteModal = false;
      this.showConversationModal = false;
      this.showEditModal = false;
    },
    findCountryFlag(countryCode, cityAndCountry) {
      try {
        const countryFlag = countryCode ? getCountryFlag(countryCode) : '🌎';
        return `${cityAndCountry} ${countryFlag}`;
      } catch (error) {
        return '';
      }
    },
    async deleteContact({ id }) {
      try {
        await this.$store.dispatch('contacts/delete', id);
        this.$emit('panel-close');
        this.showAlert(this.$t('DELETE_CONTACT.API.SUCCESS_MESSAGE'));
        if (this.$route.name !== 'contacts_dashboard') {
          this.$router.push({ name: 'contacts_dashboard' });
        }
      } catch (error) {
        this.showAlert(
          error.message
            ? error.message
            : this.$t('DELETE_CONTACT.API.ERROR_MESSAGE')
        );
      }
    },
    openMergeModal() {
      this.toggleMergeModal();
    },
  },
};
</script>

<style scoped lang="scss">
.contact--profile {
  position: relative;
  align-items: flex-start;
  padding: var(--space-normal);

  .user-thumbnail-box {
    margin-right: var(--space-normal);
  }
}

.contact--details {
  margin-top: var(--space-small);
  width: 100%;
}

.contact--info {
  text-align: left;
}

.contact--name-wrap {
  display: flex;
  align-items: center;
  margin-bottom: var(--space-small);
}

.contact--name {
  text-transform: capitalize;
  white-space: normal;
  margin: 0 var(--space-smaller) 0 0;

  a {
    color: var(--color-body);
  }
}

.contact--metadata {
  margin-bottom: var(--space-slab);
}

.contact-actions {
  margin-top: var(--space-small);
}

.contact-actions {
  display: flex;
  align-items: center;
  width: 100%;

  .new-message,
  .edit-contact,
  .merge-contact,
  .delete-contact {
    margin-right: var(--space-small);
  }
}
.merege-summary--card {
  padding: var(--space-normal);
}
.contact--bio {
  word-wrap: break-word;
}

.button--contact-menu {
  position: absolute;
  right: var(--space-normal);
  top: 0;
}
</style>
