<template>
  <div id="app" style="height:100%;">
    <div v-transfer-dom>
      <loading v-model="isLoading" delay="1"></loading>
    </div>
    <view-box ref="viewBox" body-padding-bottom="0">
      <transition
      :name="viewTransition"
      :css="!!direction">
        <router-view class="router-view"></router-view>
      </transition>

      <tabbar class="vux-demo-tabbar" icon-class="vux-center" v-show="!isTabbarDemo" slot="bottom">
        <tabbar-item :link="{path:'/home'}" :selected="route.path=='/home'">
          <span class="al al-home1 font20" slot="icon" style="position:relative;top: -2px;"></span>
          <span slot="label">{{ $t('Home') }}</span>
        </tabbar-item>
        <tabbar-item :link="{path:'/sales'}" :selected="route.path=='/sales'">
          <span class="al al-tag font20" slot="icon"></span>
          <span slot="label">{{ $t('Sales') }}</span>
        </tabbar-item>
        <tabbar-item :link="{path:'/message'}" :selected="route.path=='/message'">
          <span class="al al-mark font20" slot="icon"></span>
          <span slot="label">{{ $t('Message') }}</span>
        </tabbar-item>
        <tabbar-item :link="{path:'/favorite'}" :selected="route.path=='/favorite'">
          <span class="al al-favor font20" slot="icon"></span>
          <span slot="label">{{ $t('Favorite') }}</span>
        </tabbar-item>
        <tabbar-item :link="{path:'/center'}" :selected="route.path=='/center'">
          <span class="al al-peoplefill font20" slot="icon"></span>
          <span slot="label">{{ $t('Center') }}</span>
        </tabbar-item>
      </tabbar>

    </view-box>
  </div>
</template>

<i18n>
Home:
  zh-CN: 首页
Sales:
  zh-CN: 促销
Message:
  zh-CN: 消息
Favorite:
  zh-CN: 收藏
Center:
  zh-CN: 个人中心
</i18n>

<script>
import { ViewBox, XHeader, Loading, Tabbar, TabbarItem, TransferDom } from 'vux'
import { mapState } from 'vuex'

export default {
  name: 'app',
  directives: {
    TransferDom
  },
  components: {
    ViewBox,
    XHeader,
    Loading,
    Tabbar,
    TabbarItem
  },
  watch: {
    path (path) {
      if (path === '/component/demo') {
        this.$router.replace('/demo')
      }
    }
  },
  computed: {
    ...mapState({
      route: state => state.route,
      path: state => state.route.path,
      isLoading: state => state.vux.isLoading,
      direction: state => state.vux.direction
    }),
    leftOptions () {
      return {
        showBack: this.route.path !== '/'
      }
    },
    rightOptions () {
      return {
        showMore: true
      }
    },
    headerTransition () {
      if (!this.direction) return ''
      return this.direction === 'forward' ? 'vux-header-fade-in-right' : 'vux-header-fade-in-left'
    },
    componentName () {
      if (this.route.path) {
        const parts = this.route.path.split('/')
        if (/component/.test(this.route.path) && parts[2]) return parts[2]
      }
    },
    isDemo () {
      console.log(this.route.path)
      // return /component|demo/.test(this.route.path)
      switch (this.route.path) {
        case '/centerOperating' :
          break
        case '/centerSales' :
          break
        case '/centerService' :
          break
        default:
          return false
      }
      return true
    },
    isTabbarDemo () {
      return /tabbar/.test(this.route.path)
    },
    title () {
      if (this.route.path === '/') return 'Home'
      if (this.route.path === '/components') return 'Demo list'
      return this.componentName ? `Demo/${this.componentName}` : 'Demo/~~'
    },
    viewTransition () {
      console.log(this.direction)
      if (!this.direction) return ''
      return 'vux-' + (this.direction === 'forward' ? 'in' : 'out')
    }
  },
  created () {
    this.$http.get('https://laravel.boka.cn/api/list/news?uploader=1', {}).then(response => {
      // get status
      console.log(response.status)
      // get status text
      console.log(response.statusText)
      // get 'Expires' header
      console.log(response.headers.get('Expires'))
      // get body data
      this.someData = response.body
    }, response => {
      // error callback
    })
  }
}
</script>

<style lang="less">
@import '~vux/src/styles/reset.less';
@import '~vux/src/styles/1px.less';
@import '~vux/src/styles/tap.less';
@import '~vux/src/styles/close.less';
@import '~vux/src/styles/center.less';
@import './assets/global';
@import './assets/fonts.less';
@import './assets/hack.less';

body {
  background-color: #f7f7f7;
}
html, body {
  height: 100%;
  width: 100%;
  overflow-x: hidden;
}
.demo-icon-22 {
  font-family: 'vux-demo';
  font-size: 22px;
  color: #888;
}
.weui-tabbar.vux-demo-tabbar {
  /** backdrop-filter: blur(10px);
  background-color: none;
  background: rgba(247, 247, 250, 0.5);**/
}
.vux-demo-tabbar .weui-bar__item_on .demo-icon-22 {
  color: #F70968;
}
.vux-demo-tabbar .weui-tabbar_item.weui-bar__item_on .vux-demo-tabbar-icon-home {
  color: rgb(53, 73, 94);
}
.demo-icon-22:before {
  content: attr(icon);
}
.vux-demo-tabbar-component {
  background-color: #F70968;
  color: #fff;
  border-radius: 7px;
  padding: 0 4px;
  line-height: 14px;
}
.weui-tabbar__icon + .weui-tabbar__label {
  margin-top: 0!important;
}
.vux-demo-header-box {
  z-index: 100;
  position: absolute;
  width: 100%;
  left: 0;
  top: 0;
}
@font-face {
  font-family: 'vux-demo';  /* project id 70323 */
  src: url('https://at.alicdn.com/t/font_h1fz4ogaj5cm1jor.eot');
  src: url('https://at.alicdn.com/t/font_h1fz4ogaj5cm1jor.eot?#iefix') format('embedded-opentype'),
  url('https://at.alicdn.com/t/font_h1fz4ogaj5cm1jor.woff') format('woff'),
  url('https://at.alicdn.com/t/font_h1fz4ogaj5cm1jor.ttf') format('truetype'),
  url('https://at.alicdn.com/t/font_h1fz4ogaj5cm1jor.svg#iconfont') format('svg');
}
.demo-icon {
  font-family: 'vux-demo';
  font-size: 20px;
  color: #04BE02;
}
.demo-icon-big {
  font-size: 28px;
}
.demo-icon:before {
  content: attr(icon);
}
.router-view {
  width: 100%;
}
.vux-out-enter-active,
.vux-out-leave-active,
.vux-in-enter-active,
.vux-in-leave-active {
  transition: all 350ms;
  position: absolute;
  width: 100%;
  box-sizing: border-box;
  backface-visibility: hidden;
  perspective: 1000;
  will-change: transform;
}

.vux-out-enter {
  opacity: 0;
  transform: translate3d(-100%, 0, 0);
}

.vux-out-leave-active {
  opacity: 0;
  transform: translate3d(100%, 0, 0);
}

.vux-in-enter {
  opacity: 0;
  transform: translate3d(100%, 0, 0);
}

.vux-in-leave-active {
  opacity: 0;
  transform: translate3d(-100%, 0, 0);
}

.menu-title {
  color: #888;
}
</style>
