<template>
  <div :style="{'height':height}" class="map" id="map" v-loading="true"></div>
</template>
<script>
import star from '../../public/img/star.png' //以import的方式导入图片文件
import starRed from '../../public/img/star-red.png'

export default {
  data() {
    return {
      height: 0,
      width: 0,

      address: [
        // ['伦敦', 0.150000, 51.300000],
        ['车陂', 113.400383,23.130074, '地址：广东省广州市天河区中山大道中6号2楼'],
        ['大学城', 113.40094,23.060363, '地址：广东省广州市番禺区大学城内环东路']
      ],
      localAddress: [
        '广东省广州市'
      ]
    }
  },
  mounted() {
    this.height = window.innerHeight - 58 + 'px'
    this.$nextTick(function() {
      let _this = this
      _this.loadScript('XX997fp9jKEMv6p70M8lB4i4jIq3npRu').then(BMap => {
        var map = new BMap.Map('map')
        var point = new BMap.Point(113.331608,23.1435)
        map.centerAndZoom(point, 13)
        map.addControl(
          new BMap.NavigationControl({ type: BMAP_NAVIGATION_CONTROL_SMALL })
        )
        map.enableScrollWheelZoom(true)
        map.enableDoubleClickZoom(true)
        // map.setMapStyleV2({
        //   styleId: 'ea7b8a5ea93d9e2b21ee89764a227c4a'
        // })

        const deviceSize = new BMap.Size(24, 24) //图标大小
        function addMarker(point, tipInfo) {
          // 创建图标对象
          var myIcon = new BMap.Icon(starRed, deviceSize, {
            imageSize: deviceSize
          })
          // 创建标注对象并添加到地图
          var marker = new BMap.Marker(point, { icon: myIcon })
          map.addOverlay(marker)
          if (tipInfo) {
            // 创建信息窗口
            var opts = {
              width: 200,
              height: 100,
              title: tipInfo.title
            };
            var infoWindow = new BMap.InfoWindow(tipInfo.content, opts);
            // 点标记添加点击事件
            marker.addEventListener('click', function () {
                map.openInfoWindow(infoWindow, point); // 开启信息窗口
            });
          }
        }
        function addPoint(data) {
          let point = new BMap.Point(data[1], data[2])
          addMarker(point, { title: data[0], content: data[3] })
        }

        let myGeo = new BMap.Geocoder()
        for (let i = 0; i < _this.address.length; i++) {
          if (typeof _this.address[i] == 'object') {
            addPoint(_this.address[i])
          } else {
            myGeo.getPoint(_this.address[i], function(point) {
              if (point) {
                addMarker(point)
              }
            })
          }
        }

      
      })
    })
  },
  methods: {
    loadScript(ak) {
      return new Promise(function(resolve, reject) {
        var script = document.createElement('script')
        script.type = 'text/javascript'
        script.src =
          'https://api.map.baidu.com/api?v=3.0&ak=' + ak + '&callback=init'
        script.onerror = reject
        document.head.appendChild(script)

        if (typeof BMap != 'undefined') {
          resolve(BMap)
        } else {
          setTimeout(function() {
            resolve(BMap)
          }, 1000)
        }
      })
    }
  }
}
</script>
<style lang="scss" scoped>
.map {
  width: 100%;
}
</style>