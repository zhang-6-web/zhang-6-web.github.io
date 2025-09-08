+++
date = '2025-09-08T13:35:07+08:00'
draft = false
title = 'leaflet添加格网demo'

tags=['leaflet','Map']

categories=['leaflet']

+++

leaflet格网的作用：<font style="color:rgba(0, 0, 0, 0.9);">希望格网的间距与地图的缩放层级相关联，并且格网的大小应该类似于地图瓦片的分辨率。</font>

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Leaflet 动态格网示例</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />
    <style>
      #map {
        height: 100vh;
        width: 100%;
      }
    </style>
  </head>
  <body>
    <div id="map"></div>
    <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
    <script>
      // 初始化地图
      var map = L.map('map', {
        center: [39.9042, 116.4074], // 以北京为例
        zoom: 13,
        minZoom: 5,
        maxZoom: 18
      });
      // 添加默认瓦片图层
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
      }).addTo(map);
      // 动态绘制格网
      function drawGrid() {
        // 获取当前地图层级
        const currentZoom = map.getZoom()
        // 清除旧的格网
        map.eachLayer(function (layer) {
          if (layer instanceof L.Polyline) {
            map.removeLayer(layer);
          }
        });
        if (currentZoom >= 15) {
          return
        }
        var zoom = map.getZoom();
        var bounds = map.getBounds();
        // 计算当前缩放层级下的分辨率
        var resolution = 180 / Math.pow(2, zoom);
        // 计算格网的起始点
        var minLat = Math.floor(bounds.getSouth() / resolution) * resolution;
        var maxLat = Math.ceil(bounds.getNorth() / resolution) * resolution;
        var minLng = Math.floor(bounds.getWest() / resolution) * resolution;
        var maxLng = Math.ceil(bounds.getEast() / resolution) * resolution;
        // 经度格网
        for (var lng = minLng; lng <= maxLng; lng += resolution) {
          L.polyline([
            [minLat, lng],
            [maxLat, lng]
          ], {
            color: 'blue',
            weight: 1
          }).addTo(map);
        }
        // 纬度格网
        for (var lat = minLat; lat <= maxLat; lat += resolution) {
          L.polyline([
            [lat, minLng],
            [lat, maxLng]
          ], {
            color: 'blue',
            weight: 1
          }).addTo(map);
        }
      }
      // 初始化格网
      drawGrid();
      // 监听地图缩放事件，动态更新格网
      map.on('zoomend', drawGrid);
    </script>
  </body>

</html>
```

