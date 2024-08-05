<template>
    <div id="maps" class="maps"></div>
  </template>
  
  <script>
  import { defineComponent, onMounted } from 'vue';
  import 'ol/ol.css';
  import Map from 'ol/Map';
  import View from 'ol/View';
  import TileLayer from 'ol/layer/Tile';
  import XYZ from 'ol/source/XYZ';
  // 从ol/proj模块导入transform函数
  import { transform } from 'ol/proj';
  
  export default defineComponent({
    name: 'MapComponent',
    setup() {
      const initMap = () => {
        // 修正URL，去除末尾的多余字符
        const source = new XYZ({
          url: "http://t0.tianditu.gov.cn/img_w/wmts?SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=img&STYLE=default&TILEMATRIXSET=w&FORMAT=tiles&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}&tk=172d76ed8e929f9ae07f8a98e989fd95",
        });
    
        const layer = new TileLayer({
          source: source,
        });
  
        // 使用transform函数转换坐标
        const center = transform([116.407526, 39.90403], 'EPSG:4326', 'EPSG:3857');
    
        new Map({
          target: 'maps',
          layers: [layer],
          view: new View({
            center: center,
            zoom: 4,
            projection: 'EPSG:3857',
          }),
        });
      };
    
      onMounted(initMap);
    },
  });
  </script>
  
  <style>
  .maps {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
  }
  </style>