<template>
  <div v-on-clickaway="closeSearch" class="search-wrap">
    <div class="search-header--wrap">
      <woot-sidemenu-icon v-if="!showSearchBox" />
      <div class="search" :class="{ 'is-active': showSearchBox }" style="
        
        border-radius: 5px;
        padding-left: 2%; background-color: #f5f5f5;"
        >
        <div class="icon">
          <fluent-icon icon="search" class="search--icon" size="28" />
        </div>
        <input
          v-model="searchTerm" style="background-color: #f5f5f5; font-family: 'bipermedium'!important;"
          class="search--input"
          :placeholder="$t('CONVERSATION.SEARCH_MESSAGES')"
          @focus="onSearch"
        />
      </div>
    </div>
    <div v-if="showSearchBox" class="results-wrap">
      <div class="show-results">
        <div>
          <div class="result-view">
            <div class="result" style="font-family: 'bipermedium'!important;">
              {{ $t('CONVERSATION.SEARCH.RESULT_TITLE') }}
              <span v-if="resultsCount" class="message-counter" style="font-family: 'bipermedium'!important;">
                ({{ resultsCount }})
              </span>
            </div>
            <div v-if="uiFlags.isFetching" class="search--activity-message" style="font-family: 'biperegular'!important;">
              <woot-spinner size="" />
              {{ $t('CONVERSATION.SEARCH.LOADING_MESSAGE') }}
            </div>
          </div>

          <div v-if="showSearchResult" class="search-results--container">
            <result-item style="font-family: 'biperegular'!important;"
              v-for="conversation in conversations"
              :key="conversation.messageId"
              :conversation-id="conversation.id"
              :user-name="conversation.contact.name"
              :timestamp="conversation.created_at"
              :messages="conversation.messages"
              :search-term="searchTerm"
              :inbox-name="conversation.inbox.name"
            />
          </div>
          <div v-else-if="showEmptyResult" class="search--activity-no-message" style="font-family: 'biperegular'!important;">
            {{ $t('CONVERSATION.SEARCH.NO_MATCHING_RESULTS') }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mixin as clickaway } from 'vue-clickaway';
import { mapGetters } from 'vuex';
import timeMixin from '../../../../mixins/time';
import ResultItem from './ResultItem';
import messageFormatterMixin from 'shared/mixins/messageFormatterMixin';
import SwitchLayout from './SwitchLayout.vue';
export default {
  components: {
    ResultItem,
    SwitchLayout,
  },

  directives: {
    focus: {
      inserted(el) {
        el.focus();
      },
    },
  },

  mixins: [timeMixin, messageFormatterMixin, clickaway],

  props: {
    isOnExpandedLayout: {
      type: Boolean,
      required: true,
    },
  },

  data() {
    return {
      searchTerm: '',
      showSearchBox: false,
    };
  },

  computed: {
    ...mapGetters({
      conversations: 'conversationSearch/getConversations',
      uiFlags: 'conversationSearch/getUIFlags',
      currentPage: 'conversationPage/getCurrentPage',
    }),
    resultsCount() {
      return this.conversations.length;
    },
    showSearchResult() {
      return (
        this.searchTerm && this.conversations.length && !this.uiFlags.isFetching
      );
    },
    showEmptyResult() {
      return (
        this.searchTerm &&
        !this.conversations.length &&
        !this.uiFlags.isFetching
      );
    },
  },

  watch: {
    searchTerm(newValue) {
      if (this.typingTimer) {
        clearTimeout(this.typingTimer);
      }

      this.typingTimer = setTimeout(() => {
        this.hasSearched = true;
        this.$store.dispatch('conversationSearch/get', { q: newValue });
      }, 1000);
    },
    currentPage() {
      this.clearSearchTerm();
    },
  },

  mounted() {
    this.$store.dispatch('conversationSearch/get', { q: '' });
    bus.$on('clearSearchInput', () => {
      this.clearSearchTerm();
    });
  },

  methods: {
    onSearch() {
      this.showSearchBox = true;
    },
    closeSearch() {
      this.showSearchBox = false;
    },
    clearSearchTerm() {
      this.searchTerm = '';
    },
  },
};
</script>

<style lang="scss" scoped>
.search-wrap {
  position: relative;
  padding: var(--space-one) var(--space-normal) var(--space-smaller)
    var(--space-normal);
}

.search-header--wrap {
  display: flex;
  justify-content: space-between;
  min-height: var(--space-large);
}

.search {
  display: flex;
  flex: 1;
  padding: 0;
  border-bottom: 1px solid transparent;

  &:hover {
    .search--icon {
      color: #000;
    }
  }
}
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@500;600;700;800&display=swap');
.search--input {
  align-items: center;
  border: 0;
  color: var(--color-body);
  cursor: pointer;
  width: 100%;
  display: flex;
  font-size: 18px;
  font-weight: 500!important;
  text-align: left;
  font-family: 'bipermedium'!important;
  line-height: var(--font-size-large);
}

input::placeholder{
  font-size: 15px!important;
  color: #000!important;
  margin-top: -5px!important;
  font-family: 'bipermedium'!important;
}

.search--icon {
  color: var(--s-600);
  font-size: var(--font-size-large);
  padding: 0 var(--space-small) 0 0;
}

.icon {
  display: flex;
}


.results-wrap {
  position: absolute;
  z-index: 9999;
  box-shadow: var(--shadow-large);
  background: white;
  width: 100%;
  max-height: 70vh;
  overflow: auto;
  left: 0;
}

.show-results {
  list-style-type: none;
  font-size: var(--font-size-small);
  font-weight: var(--font-weight-normal);
}

.result-view {
  display: flex;
  justify-content: space-between;
}

.result {
  padding: var(--space-smaller) var(--space-smaller) var(--space-smaller)
    var(--space-normal);
  color: var(--s-700);
  font-size: var(--font-size-medium);
  font-weight: var(--font-weight-bold);

  .message-counter {
    color: var(--s-500);
    font-size: var(--font-size-small);
    font-weight: var(--font-weight-bold);
  }
}

.search--activity-message {
  padding: var(--space-small) var(--space-normal) var(--space-small)
    var(--space-zero);
  font-size: var(--font-size-small);
  font-weight: var(--font-weight-medium);
  color: var(--s-500);
}

.search--activity-no-message {
  display: flex;
  justify-content: center;
  padding: var(--space-one) var(--space-zero) var(--space-two) var(--space-zero);
  font-size: var(--font-size-small);
  font-weight: var(--font-weight-medium);
  color: var(--s-500);
}
</style>
