<template>
  <div id="map" style="width:100%;height:500px;"></div>
</template>

<script>

import '../../node_modules/ol/ol.css'
import TileLayer from "ol/layer/Tile";
import XYZ from "ol/source/XYZ";
import Map from "ol/Map";
import ol, {View} from "ol";
import { createStringXY } from 'ol/coordinate.js';
import {control,default as defaultControls, ZoomSlider,ZoomToExtent} from "ol/control";
import MousePosition from "ol/control/MousePosition"
// import  from 'ol/control/ZoomSlider'

export default {
  // name: 'map',
  mounted() {
    this.initMap()
  },
  data () {
    return {
      map: null,
      url: 'https://webrd01.is.autonavi.com/appmaptile?lang=zh_cn&size=1&scale=1&style=8&x={x}&y={y}&z={z}', // 加载在线瓦片地图
      // 地图View展示选项配置
      viewOptions: {
        projection: 'EPSG:4326',
        center: [104.089175, 30.650451], // [104.089175, 30.650451] 地图中心位置
        zoom: 8
        // extent: [101.9004807124176, 28.837774208959473, 103.38848561448468, 30.9210929107385], // [minX, minY, maxX, maxY]
        // minZoom: 6,　
        // maxZoom: 19 // 缩放最大级别控制
      },
      view: null,
      source: null,
      pielayer: null, // 点线信息的图层
      MousePosition:{
      //坐标格式,坐标保留4位小数
      coordinateFormat: createStringXY(4),
        //地图投影坐标系（若未设置则输出为默认投影坐标系下的坐标）
        //projection: 'EPSG:4326',
        //坐标信息显示样式类名，默认是'ol-mouse-position'
        className: 'custom-mouse-position',
      //显示鼠标位置信息的目标容器
      target: document.getElementById('mouse-position'),
      //未定义坐标的标记
      undefinedHTML: '&nbsp;'
    }
    }
  },
  methods :{
    initMap () {
      // let _this = this
      var layers = new TileLayer({
        source: new XYZ({
          url: this.url
        })
      })
      var zoomToExtent = new ZoomToExtent({
        extent: [
          813079.7791264898, 5929220.284081122,
          848966.9639063801, 5936863.986909639
        ]
      })
      var mousePositionControl = new MousePosition(this.MousePosition)
      this.view = new View(this.viewOptions)
      this.map = new Map({
        layers: [ layers ],
        target: 'map',
        view: this.view
      })
      var zoomslider = new ZoomSlider()
      this.map.addControl(zoomslider);
      this.map.addControl(zoomToExtent);
      this.map.addControl(mousePositionControl);

    }
  }
}
</script>

<style scoped>
/*#map {*/
/*  width: 100%;*/
/*  height: 95%;*/
/*  position: absolute;*/
/*}*/
/* 缩放滑块控件ZoomSlider的样式，放置到缩放按钮之间实现导航条功能*/
/*未起作用*/
#map .ol-zoom .ol-zoom-out {
  margin-top: 204px;
}

#map .ol-zoomslider {
  background-color: transparent;
  top: 2.3em;
}

#map .ol-touch .ol-zoom .ol-zoom-out {
  margin-top: 212px;
}

#map .ol-touch .ol-zoomslider {
  top: 2.75em;
}

#map .ol-zoom-in .ol.has-tooltip:hover[role=tooltip],
#map .ol-zoom-in .ol-has-tooltip:focus[role=tooltip] {
  top: 3px;
}

#map .ol-zoom-out .ol-has-tooltip:hover[role=tooltip],
#map .ol-zoon-out .ol-has-out-tooltip:focus[role=tooltip] {
  top: 232px;
}

#map .ol-zoom-extent {
  top: 280px;
}
</style>
