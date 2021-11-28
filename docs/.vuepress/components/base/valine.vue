<template>
  <div :id="`valine-wrap-${uuid}`" class="valine-module"></div>
</template>

<script>
export default {
  name: 'Valine',
  data() {
    return {
      uuid: new Date().getTime() + Math.round(Math.random() * 10000)
    }
  },
  mounted: function() {
    // require window
    const Valine = require('valine')
    if (typeof window !== 'undefined') {
      this.window = window
      window.AV = require('leancloud-storage')
    }

    new Valine({
      el: `#valine-wrap-${this.uuid}`,
      appId: 'eyxsCniVynwRNPHjkKOaU3rv-gzGzoHsz', // your LeanCloud appId
      appKey: '1Ulln6hepA0i1UKvXKUVdTAG', // your LeanCloud appKey
      notify: false,
      verify: false,
      avatar: 'retro',
      placeholder: '你想说点什么呢...',
      visitor: true,
      recordIP: true,
      meta: ['nick', 'mail'],
      path: window.location.pathname
    })
  }
}
</script>
<style lang="scss">
.valine-module {
  margin: 50px 0;
  .vcopy.txt-right {
    display: none;
  }
}
</style>