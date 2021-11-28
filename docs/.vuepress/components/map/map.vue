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
        ['伦敦', 0.150000, 51.300000],
        ['香港', 114.186000, 22.250000],
        ['澳门', 113.575050, 22.150130],
        

        // '广东省广州市',
        '广东省深圳市',
        '广东省韶关市',
        '广东省佛山市',
        '广东省中山市',
        '广东省珠海市',
        '广东省江门市',
        '广东省阳江市',
        '广东省湛江市',
        '北京市',
        '重庆市解放碑',
        '上海市',
        '浙江省杭州市',
        '浙江省嘉兴市',
        '湖南省长沙市',
        '湖南省株洲市',
        '湖南省衡阳市',
        '江苏省南京市',
        '云南省昆明市',
        '山东省临沂市',
        '广西省北海市',
        // '澳门特别行政区'
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
        var point = new BMap.Point(111.19, 32.26)
        map.centerAndZoom(point, 6)
        map.addControl(
          new BMap.NavigationControl({ type: BMAP_NAVIGATION_CONTROL_SMALL })
        )
        map.enableScrollWheelZoom(true)
        map.enableDoubleClickZoom(true)
        map.setMapStyleV2({
          styleId: 'ea7b8a5ea93d9e2b21ee89764a227c4a'
        })

        const deviceSize = new BMap.Size(17, 17) //图标大小
        function addMarker(point) {
          // 创建图标对象
          var myIcon = new BMap.Icon(star, deviceSize, {
            imageSize: deviceSize
          })
          // 创建标注对象并添加到地图
          var marker = new BMap.Marker(point, { icon: myIcon })
          map.addOverlay(marker)
        }
        function addPoint(data) {
          let point = new BMap.Point(data[1], data[2])
          addMarker(point)
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

        //添加广州坐标 
        function addMarker1(point) {
          // 创建图标对象
          var myIcon1 = new BMap.Icon(starRed, deviceSize, {
            imageSize: deviceSize
          })
          // 创建标注对象并添加到地图
          var marker1 = new BMap.Marker(point, { icon: myIcon1 })
          map.addOverlay(marker1)
        }
        let myGeo1 = new BMap.Geocoder()
        for (let i = 0; i < _this.localAddress.length; i++) {
            myGeo1.getPoint(_this.localAddress[i], function(point) {
              if (point) {
                addMarker1(point)
              }
            })
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