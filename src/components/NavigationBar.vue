<template>
  <div class="navigation-bar" :class="{ dark: isDarkTheme }">
    <div class="button-group">
      <a class="avatar">
        <img :src="avatarURL" alt="">
      </a>
      <fa-icon iconName="arrow-left" :style="faStyle" :iconStyle="iconStyle" :click="goBack" :highlightedStyle="highlightedStyle"/>
      <fa-icon iconName="home" :style="faStyle" :iconStyle="iconStyle" :click="goHome" :highlighted="routeHome" :highlightedStyle="highlightedStyle" />
      <fa-icon iconName="user" :style="faStyle" :iconStyle="iconStyle" :click="goAbout" :highlighted="routeProfile" :highlightedStyle="highlightedStyle" />
      <fa-icon iconName="inbox" :style="faStyle" :iconStyle="iconStyle" :highlightedStyle="highlightedStyle"/>
      <fa-icon iconName="bar-chart" :style="faStyle" :iconStyle="iconStyle" :highlightedStyle="highlightedStyle"/>
      <fa-icon iconName="search" :style="faStyle" :iconStyle="iconStyle" :highlightedStyle="highlightedStyle"/>
    </div>
    <div class="button-group">
      <fa-icon iconName="pencil-square-o" :style="faStyle" :iconStyle="highlightIconStyle" :click="newPlurk" />
      <fa-icon :iconName="toggleIconName" :style="faStyle" :iconStyle="iconStyle" :click="toggleStyle"/>
    </div>

  </div>
</template>

<script>
import { mapGetters, mapState, mapActions } from 'vuex';

import FaIcon from 'components/FaIcon.vue';
import { avatarURL } from 'helpers/userHelper';
import { openCreatePlurkPopoup } from 'utils/ipcActions';

export default {
  name: 'NavigationBar',

  components: {
    FaIcon
  },

  methods: {
    goBack() {
      this.$router.go(-1);
    },

    goHome() {
      this.$router.push({ name: 'timeline' });
    },

    goAbout() {
      this.user && this.$router.push({ name: 'about', params: { user_id: this.user.id } });
    },

    newPlurk() {
      openCreatePlurkPopoup();
    },

    ...mapActions([
      'toggleStyle'
    ])
  },

  computed: {
    avatarURL() {
      return avatarURL(this.user);
    },

    routeHome() {
      return this.$route.path === '/';
    },

    routeProfile() {
      return this.user && this.$route.path === `/about/${this.user.id}`;
    },

    isDarkTheme() {
      return this.theme === 'dark';
    },

    toggleIconName() {
      return this.isDarkTheme ? 'toggle-on' : 'toggle-off';
    },

    highlightIconStyle() {
      return { ...this.iconStyle, ...this.highlightedStyle };
    },

    ...mapGetters({
      user: 'currentUser'
    }),

    ...mapState({
      theme: state => state.appTheme
    })
  },

  data() {
    return {
      faStyle: {
        width: '1.5em',
        margin: '0 auto',
        textAlign: 'center'
      },

      iconStyle: {
        color: '#8899a6',
        fontSize: '1.5em',
        margin: '.5em 0'
      },

      highlightedStyle: {
        color: '#f6882d'
      }
    };
  }
}
</script>

<style lang="sass" scoped>
.navigation-bar	{
  height: 100%;
  padding-top: 3em;
  padding-bottom: 1em;
  min-width: 75px;
  max-width: 75px;

  background-color: rgb(245, 231, 223);

  &.dark {
    background-color: #26323f;
  }

  -webkit-app-region: drag;

  display: flex;
  flex-direction: column;
  justify-content: space-between;
  box-sizing: border-box;

  .button-group {
    display: flex;
    flex-direction: column;
  }

  a.avatar {
    padding: 10px;
    max-width: 100%;
    text-align: center;

    img {
      max-width: 100%;
      border-radius: 5px;
    }
  }
}
</style>
