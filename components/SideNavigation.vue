<template>
  <div class="SideNavigation">
    <div class="SideNavigation-HeadingContainer sp-flex">
      <v-icon
        class="SideNavigation-HeadingIcon pc-none"
        :aria-label="$t('Navi Open')"
        @click="openNavi"
      >
        mdi-menu
      </v-icon>
      <nuxt-link to="/" class="SideNavigation-HeadingLink">
        <div class="SideNavigation-Logo">
          <img src="/logo.svg" :alt="$t('Tokyo')" />
        </div>
        <h1 class="SideNavigation-Heading">
          {{ $t('COVID-19') }}<br />{{ $t('Measures site (Unofficial)') }}
        </h1>
      </nuxt-link>
    </div>
    <v-divider class="SideNavigation-HeadingDivider" />
    <div class="sp-none" :class="{ open: isNaviOpen }">
      <v-icon
        class="SideNavigation-ListContainerIcon pc-none"
        :aria-label="$t('Navi Close')"
        @click="closeNavi"
      >
        mdi-close
      </v-icon>
      <v-list :flat="true">
        <v-container
          v-for="(item, i) in items"
          :key="i"
          class="SideNavigation-ListItemContainer"
          @click="closeNavi"
        >
          <ListItem :link="item.link" :icon="item.icon" :title="item.title" />
          <v-divider v-show="item.divider" class="SideNavigation-Divider" />
        </v-container>
      </v-list>
      <div class="SideNavigation-Footer">
        <div class="SideNavigation-SocialLinkContainer">
          <a
            href="https://twitter.com/stopcovid19ai"
            target="_blank"
            rel="noopener"
          >
            <img src="/twitter.png" alt="Twitter" />
          </a>
          <a
            href="https://github.com/code4nagoya/covid19"
            target="_blank"
            rel="noopener"
          >
            <img src="/github.png" alt="GitHub" />
          </a>
        </div>
        <div class="SideNavigation-SponsorLinkContainer">
          {{ $t('Powered by:') }}<br />
          <a href="https://www.sakura.ad.jp/" target="_blank" rel="noopener">
            <span class="image-title">{{ $t('SAKURA internet Inc.') }}</span>
            <img
              class="sakura-internet-logo"
              src="/sakura-internet.svg"
              width="172px"
              height="46.5px"
              :alt="$t('SAKURA internet Inc.')"
            />
          </a>
        </div>
        <!-- <small class="SideNavigation-Copyright" lang="en">
          Copyright &copy; 2020 Tokyo Metropolitan Government. All Rights
          Reserved.
        </small> -->
      </div>
    </div>
  </div>
</template>

<i18n>
{
  "ja": {
    "Navi Open": "サイドメニュー項目を開く",
    "Navi Close": "サイドメニュー項目を閉じる",
    "Aichi": "愛知県",
    "COVID-19": "新型コロナウイルス感染症",
    "Measures site (Unofficial)": "対策サイト(非公式)",
    "Aichi Prefecture Government": "愛知県",
    "Tokyo COVID-19 Task Force": "新型コロナウイルス感染症対策本部",
    "The latest updates": "県内の最新感染動向",
    "If you have any symptoms": "新型コロナウイルス感染症が心配なときに",
    "for Families with children": "お子様をお持ちの皆様へ",
    "for Citizens": "県民の皆様へ",
    "for Enterprises and Employees": "企業の皆様・はたらく皆様へ",
    "Official statements from Task Force": "愛知県新型コロナウイルス感染症対策本部会議報",
    "Cancelled public events": "愛知労働局主催等 中止又は延期するイベント等",
    "Government official website": "愛知県公式ホームページ",
    "Government official LINE": "愛知県-新型コロナ対策パーソナルサポート(LINE)",
    "Government official Twitter": "愛知県庁公式Twitter",
    "Message from Governor Omura": "知事からのメッセージ",
    "About us": "当サイトについて",
    "Other local Government": "他自治体の対策サイト",
    "Powered by:": "Powered by:",
    "SAKURA internet Inc.": "さくらインターネット"
  }
}
</i18n>

<script>
import ListItem from '@/components/ListItem'

export default {
  components: {
    ListItem
  },
  props: {
    isNaviOpen: {
      type: Boolean,
      required: true
    }
  },
  computed: {
    items() {
      return [
        {
          icon: 'mdi-chart-timeline-variant',
          title: this.$t('The latest updates'),
          link: '/',
          divider: true
        },
        {
          icon: 'covid',
          title: this.$t('If you have any symptoms'),
          link: '/flow',
          divider: true
        },
        {
          icon: 'parent',
          title: this.$t('for Families with children'),
          link: '/parent'
        },
        {
          icon: 'mdi-account-multiple',
          title: this.$t('for Citizens'),
          link:
            'https://www.pref.aichi.jp/site/covid19-aichi/kansensya-kensa.html'
        },
        {
          icon: 'mdi-domain',
          title: this.$t('for Enterprises and Employees'),
          link: '/worker',
          divider: true
        },
        {
          title: this.$t('Official statements from Task Force'),
          link:
            'https://www.pref.aichi.jp/site/covid19-aichi/novel-coronavirus-taisakuhonbu.html'
        },
        {
          title: this.$t('Cancelled public events'),
          link:
            'https://jsite.mhlw.go.jp/aichi-roudoukyoku/news_topics/2019_covid-19_event.html'
        },
        {
          title: this.$t('Message from Governor Omura'),
          link: 'https://www.pref.aichi.jp/chiji/message/index00.html'
        },
        {
          title: this.$t('About us'),
          link: '/about'
        },
        {
          title: this.$t('Government official website'),
          link: 'https://www.pref.aichi.jp/'
        },
        {
          title: this.$t('Government official LINE'),
          link: 'https://liny.link/r/1653954481-KA3xJl9g?lp=8Zhyvz'
        },
        {
          title: this.$t('Government official Twitter'),
          link: 'https://twitter.com/pref_aichi',
          divider: true
        },
        {
          title: this.$t('Other local Government'),
          link: 'https://stopcovid19.code4japan.org/'
        }
      ]
    },
    isClass() {
      return item => (item.title.charAt(0) === '【' ? 'kerningLeft' : '')
    }
  },
  methods: {
    openNavi() {
      this.$emit('openNavi')
    },
    closeNavi() {
      this.$emit('closeNavi')
    }
  }
}
</script>

<style lang="scss" scoped>
.SideNavigation {
  position: relative;
  height: 100%;
  background: $white;
  box-shadow: 0px 0px 2px rgba(0, 0, 0, 0.15);
  &-HeadingContainer {
    padding: 1.25em 0 1.25em 1.25em;
    align-items: center;
    @include lessThan($small) {
      padding: 7px 0 7px 20px;
    }
  }
  &-HeadingIcon {
    margin-right: 16px;
  }
  &-HeadingLink {
    @include lessThan($small) {
      display: flex;
      align-items: center;
    }
    text-decoration: none;
  }
  &-ListContainerIcon {
    margin: 24px 16px 0;
  }
  &-ListItemContainer {
    padding: 2px 20px;
  }
  &-Logo {
    margin: 20px 16px 0 0;
    width: 110px;
    @include lessThan($small) {
      margin-top: 0;
    }
  }
  &-Heading {
    margin-top: 8px;
    font-size: 13px;
    color: #898989;
    padding: 0.5em 0;
    text-decoration: none;
    @include lessThan($small) {
      margin-top: 0;
    }
  }
  &-HeadingDivider {
    margin: 0px 20px 4px;
    @include lessThan($small) {
      display: none;
    }
  }
  &-Divider {
    margin: 12px 0;
  }
  &-Footer {
    padding: 20px;
    background-color: $white;
  }
  &-SocialLinkContainer {
    display: flex;
    & img {
      width: 30px;
      &:first-of-type {
        margin-right: 10px;
      }
    }
  }
  &-SponsorLinkContainer {
    overflow: visible;
    padding-top: 0.8rem;
    white-space: normal;
    font-size: 0.82rem;
    color: $gray-1;
    & a {
      color: #333;
      text-decoration: none;
    }
    & a:hover {
      opacity: 0.6;
    }
    & img {
      padding-bottom: 0.9rem;
    }
    & img.sakura-internet-logo {
      margin: -6px 0 0 -14px;
      width: 176px;
    }
    & .image-title {
      display: inline-block;
      width: 0;
      height: 1.5rem;
      overflow: hidden;
    }
    & .no-image-title {
      display: inline-block;
      line-height: 1.8rem;
      color: #444;
      font-size: 1.5rem;
      font-weight: 400;
    }
  }
  &-Copyright {
    display: block;
    margin-top: 10px;
    font-size: 8px;
    line-height: 1.2;
    color: $gray-1;
    font-weight: bold;
  }
}
.open {
  @include lessThan($small) {
    position: fixed;
    overflow: auto;
    top: 0;
    bottom: 0;
    left: 0;
    display: block !important;
    width: 100%;
    z-index: z-index-of(opened-side-navigation);
    background-color: $white;
  }
}
@include largerThan($small) {
  .pc-none {
    display: none;
  }
}
@include lessThan($small) {
  .sp-flex {
    display: flex;
  }
  .sp-none {
    display: none;
  }
}
</style>
