<template>
  <div class="echarts">
    <div :style="{height: '700px', width: '100%'}" id="map" ref="chart"></div>
  </div>
</template>

<script>
import echarts from 'echarts'
import 'echarts/extension/bmap/bmap.js'
import 'echarts/map/js/china'
import { MP } from '../../static/baiduMap.js'
import { onMounted, nextTick } from 'vue'

import data from '../../static/data.json'
import geoCoordMap from '../../static/geoCoordMap.json'

const convertData = function (data) {
  const res = []
  for (let i = 0; i < data.length; i++) {
    const geoCoord = geoCoordMap[data[i].name]
    if (geoCoord) {
      geoCoord.push(data[i].confirm)
      res.push({
        name: data[i].name,
        value: geoCoord.concat(data[i].heal)
      })
    }
  }
  return res
}
const initEchart = content => {
  const myChart = echarts.init(document.getElementById('map'))

  const option = {
    tooltip: {
      trigger: 'item',
      formatter: function (param) {
        return (
          param.name +
          '</br>' +
          '累计确诊: ' +
          param.value[2] +
          '</br> 累计治愈：' +
          param.value[3]
        )
      }
    },
    bmap: {
      // 当前视角中心位置的坐标
      center: [108, 36],
      zoom: 5, // zoom 默认情况下地图缩放级别
      roam: true, // roam表示用户是否可以拖放和缩放地图。
      // 绘制地图的相关样式
      mapStyle: {
        styleJson: [
          {
            featureType: 'land',
            elementType: 'all',
            stylers: {
              color: '#f3f3f3'
            }
          },
          {
            featureType: 'railway',
            elementType: 'all',
            stylers: {
              visibility: 'off'
            }
          },
          {
            featureType: 'highway',
            elementType: 'all',
            stylers: {
              color: '#fdfdfd'
            }
          },
          {
            featureType: 'arterial',
            elementType: 'geometry.fill',
            stylers: {
              color: '#fefefe'
            }
          },
          {
            featureType: 'poi',
            elementType: 'all',
            stylers: {
              visibility: 'off'
            }
          },
          {
            featureType: 'green',
            elementType: 'all',
            stylers: {
              visibility: 'off'
            }
          },
          {
            featureType: 'subway',
            elementType: 'all',
            stylers: {
              visibility: 'off'
            }
          },
          {
            featureType: 'manmade',
            elementType: 'all',
            stylers: {
              color: '#d1d1d1'
            }
          },
          {
            featureType: 'local',
            elementType: 'all',
            stylers: {
              color: '#d1d1d1'
            }
          },
          {
            featureType: 'arterial',
            elementType: 'labels',
            stylers: {
              visibility: 'off'
            }
          },
          {
            featureType: 'boundary', // 边境线
            elementType: 'all',
            stylers: {
              color: '#828585'
            }
          },
          {
            featureType: 'building',
            elementType: 'all',
            stylers: {
              color: '#d1d1d1'
            }
          },
          {
            featureType: 'label', // 地名
            elementType: 'labels.text.fill',
            stylers: {
              color: '#999999'
            }
          }
        ]
      }
    },
    series: [
      {
        type: 'scatter', // 散点图
        coordinateSystem: 'bmap',
        data: convertData(data),
        symbolSize: function (val) {
          if (val[2] > 1 && val[2] < 50) {
            return val[2] / 4
          } else if (val[2] > 50 && val[2] < 1000) {
            return val[2] / 10
          } else if (val[2] > 1000 && val[2] < 5000) {
            return val[2] / 50
          } else {
            return val[2] / 500
          }
        },
        label: {
          formatter: '{b}',
          position: 'right',
          show: false
        },
        emphasis: {
          label: {
            show: true
          }
        }
      }
    ],
    visualMap: {
      type: 'continuous',
      min: 0,
      right: 'right',
      text: ['累计确诊人数'],
      max: 1000,
      calculable: true,
      inRange: {
        color: ['#2a71db', '#eac736', '#d94e5d']
      },
      textStyle: {
        color: 'black'
      }
    }
  }
  if (option && typeof option === 'object') {
    myChart.setOption(option)
  }
}

const ak = 'dRaHGlPDeqxyDvBrh3lRZvvjrlETNHpL'

export default {
  name: 'Map',

  setup () {
    onMounted(() => {
      nextTick(() => {
        MP(ak).then(BMap => {
          document.getElementById('map').style.height =
            window.screen.availHeight * 1 + 'px'
          echarts.init(document.getElementById('map')).resize()
        })
      })
      initEchart()
    })
    return {}
  }
}
</script>

<style lang="scss" scoped>
</style>
