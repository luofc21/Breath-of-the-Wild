<template>
  <div
    :class="pageClasses"
    @touchend="onTouchEnd"
    @touchstart="onTouchStart"
    class="theme-container"
  >
    <LoadingPage :show="loading" />

    <Navbar @toggle-sidebar="toggleSidebar" v-if="shouldShowNavbar" />

    <div @click="toggleSidebar(false)" class="sidebar-mask"></div>

    <Sidebar :items="sidebarItems" @toggle-sidebar="toggleSidebar">
      <slot #top name="sidebar-top" />
      <slot #bottom name="sidebar-bottom" />
    </Sidebar>

    <Lock v-if="haveLock()" />
    <HidePassword @unLock="unLock" />

    <Home v-if="$page.frontmatter.home" />

    <Page :sidebar-items="sidebarItems" v-if="pageVisible">
      <slot #top name="page-top" />
      <slot #bottom name="page-bottom" />
    </Page>

    <div class="record-wrap" v-if="pageVisible">
      <!-- <a href="http://beian.miit.gov.cn/">粤ICP备xxxxxxxx号</a> -->
      <div class="avatar-info">
        <i class="shni shn-heart-fill"></i>
        <span class="copyright">By LFC</span>
      </div>
      
    </div>
  </div>
</template>

<script>
import Home from '@theme/components/Home.vue'
import Navbar from '@theme/components/Navbar.vue'
import Page from '@theme/components/Page.vue'
import Sidebar from '@theme/components/Sidebar.vue'
import { resolveSidebarItems } from '../util'
import LoadingPage from '../components/LoadingPage.vue'
import Lock from '../components/Lock.vue'
import HidePassword from '../components/HidePassword.vue'

export default {
  components: { Home, Page, Sidebar, Navbar, LoadingPage, Lock, HidePassword },

  data() {
    return {
      isSidebarOpen: false,
      loading: true,

      pageVisible: true
    }
  },

  computed: {
    shouldShowNavbar() {
      const { themeConfig } = this.$site
      const { frontmatter } = this.$page
      if (frontmatter.navbar === false || themeConfig.navbar === false) {
        return false
      }
      return (
        this.$title ||
        themeConfig.logo ||
        themeConfig.repo ||
        themeConfig.nav ||
        this.$themeLocaleConfig.nav
      )
    },

    shouldShowSidebar() {
      const { frontmatter } = this.$page
      return (
        !frontmatter.home &&
        frontmatter.sidebar !== false &&
        this.sidebarItems.length
      )
    },

    sidebarItems() {
      return resolveSidebarItems(
        this.$page,
        this.$page.regularPath,
        this.$site,
        this.$localePath
      )
    },

    pageClasses() {
      const userPageClass = this.$page.frontmatter.pageClass
      return [
        {
          'no-navbar': !this.shouldShowNavbar,
          'sidebar-open': this.isSidebarOpen,
          'no-sidebar': !this.shouldShowSidebar
        },
        userPageClass
      ]
    }
  },
  watch: {
    $route: {
      handler: function(to, from) {
        if (this.$page.frontmatter.home) {
          this.pageVisible = false
        } else {
          if (this.haveLock()) {
            this.pageVisible = false
          } else {
            this.pageVisible = true
          }
        }
      },
      immediate: true
    }
  },
  mounted() {
    this.$router.afterEach(() => {
      this.isSidebarOpen = false
    })

    let _this = this

    setTimeout(() => {
      _this.loading = false
    }, 2000)
  },

  methods: {
    toggleSidebar(to) {
      this.isSidebarOpen = typeof to === 'boolean' ? to : !this.isSidebarOpen
      this.$emit('toggle-sidebar', this.isSidebarOpen)
    },

    // side swipe
    onTouchStart(e) {
      this.touchStart = {
        x: e.changedTouches[0].clientX,
        y: e.changedTouches[0].clientY
      }
    },

    onTouchEnd(e) {
      const dx = e.changedTouches[0].clientX - this.touchStart.x
      const dy = e.changedTouches[0].clientY - this.touchStart.y
      if (Math.abs(dx) > Math.abs(dy) && Math.abs(dx) > 40) {
        if (dx > 0 && this.touchStart.x <= 80) {
          this.toggleSidebar(true)
        } else {
          this.toggleSidebar(false)
        }
      }
    },

    unLock() {
      this.pageVisible = true
    },
    haveLock() {
      if (this.$store.state.lock) {
        let haveLockPath = [
          '/life/plan/2018.html',
          '/life/plan/2019.html',
          '/life/plan/2020.html',
          '/life/diary/2019-03.html',
          '/life/diary/2019-05.html',
          '/life/diary/2019-09.html',
          '/life/diary/2019-10.html',
          '/life/diary/2019-11.html',
          '/life/diary/2020-03.html',
          '/life/diary/2020-04.html',
          '/life/diary/2020-05.html'
        ]

        if (haveLockPath.indexOf(this.$route.path) > -1) {
          return true
        } else {
          return false
        }
      } else {
        return false
      }
    }
  }
}
</script>
<style lang="scss">
.record-wrap {
  z-index: 9998;
  position: relative;
  text-align: center;
  padding: 5px 0;
  background: #171d20;
  font-size: 12px;
  a {
    color: #fff;
    font-weight: 400;
  }
  // position: absolute;
  // bottom: 0;
  
  .avatar-info {
    display: flex;
    justify-content: center;
    align-items: center;
    img {
      width: 50px;
      height: 50px;
      border-radius: 50%;
      border: #fff 2px solid;
      box-shadow: 1px 1px 1px 1px rgba(0, 0, 0, 0.1);
      margin: 0 20px;
    }
    i {
      color: #f56c6c;
      animation: xintiao 1.33s ease-in-out infinite;
    }
    .copyright {
      margin-left: 10px;
      color: #FFFFFF;
    }
    @keyframes xintiao {
      0% {
        -webkit-transform: scale(1);
        transform: scale(1);
      }

      50% {
        -webkit-transform: scale(0.8);
        transform: scale(0.8);
      }

      to {
        -webkit-transform: scale(1);
        transform: scale(1);
      }
    }
  }
}
</style>
