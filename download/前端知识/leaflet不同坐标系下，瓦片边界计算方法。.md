# leaflet不同坐标系下，瓦片边界计算方法。

:::info
<font style="color:rgb(64, 64, 64);">在 Web 地图开发中，瓦片（Tile）是地图数据的基本单位。不同坐标系（如 </font>**<font style="color:rgb(64, 64, 64);">EPSG:3857</font>**<font style="color:rgb(64, 64, 64);"> 和 </font>**<font style="color:rgb(64, 64, 64);">EPSG:4326</font>**<font style="color:rgb(64, 64, 64);">）的瓦片边界计算方式不同，直接影响地图渲染和数据查询的准确性。本文档详细说明两种坐标系下的瓦片边界计算方法，并提供代码实现。</font>

:::

**<font style="color:rgb(64, 64, 64);">坐标系简介：</font>**

| **<font style="color:rgb(64, 64, 64);">坐标系</font>** | **<font style="color:rgb(64, 64, 64);">名称</font>** | **<font style="color:rgb(64, 64, 64);">单位</font>** | **<font style="color:rgb(64, 64, 64);">适用范围</font>** | **<font style="color:rgb(64, 64, 64);">特点</font>** |
| --- | --- | --- | --- | --- |
| **<font style="color:rgb(64, 64, 64);">EPSG:3857</font>** | <font style="color:rgb(64, 64, 64);">Web 墨卡托</font> | <font style="color:rgb(64, 64, 64);">米（meters）</font> | <font style="color:rgb(64, 64, 64);">Google Maps、OpenStreetMap</font> | <font style="color:rgb(64, 64, 64);">适用于 Web 地图，高纬度地区变形严重</font> |
| **<font style="color:rgb(64, 64, 64);">EPSG:4326</font>** | <font style="color:rgb(64, 64, 64);">WGS84 经纬度</font> | <font style="color:rgb(64, 64, 64);">度（degrees）</font> | <font style="color:rgb(64, 64, 64);">传统 GIS 系统</font> | <font style="color:rgb(64, 64, 64);">直接使用经纬度，无投影变形</font> |


**<font style="color:rgb(64, 64, 64);">瓦片边界计算：</font>**

<details class="lake-collapse"><summary id="u92dfbff7"><span class="ne-text" style="font-size: 16px">EPSG:4326(WGS84经纬度)</span></summary><ol class="ne-ol"><li id="ue2035c3c" data-lake-index-type="0"><span class="ne-text" style="font-size: 16px">计算瓦片的像素坐标：瓦片大小固定为256px,计算左上角和右下角的像素坐标。</span></li><li id="u1562db37" data-lake-index-type="0"><span class="ne-text" style="font-size: 16px">转换为经纬度：使用leaflet的unproject方法将像素坐标转换为经纬度。</span></li></ol><pre data-language="javascript" id="LpUSJ" class="ne-codeblock language-javascript"><code>tileLatLonBounds(x,y,zoom){
  const map=L.map('map',{
    // 使用4326坐标系
  }
  const tilesize=256;
  const nwPoint=L.point(x*tileSize,(y+1)*tileSize);
  const sePoint=L.point((x+1)*tilesize,y*tilesize);
  const nw=map.unproject(nwPoint,zoom)
  const se=map.unproject(sePoint,zoom);
  return {
   x1:nw.lng;
   y1:nw.lat;
  x2:se.lng;
  y2:se.lat;

  }

}</code></pre></details>
<details class="lake-collapse"><summary id="u96756d5a"><span class="ne-text" style="font-size: 16px">EPSG:3857(Web墨卡托)</span></summary><ol class="ne-ol"><li id="u464eec28" data-lake-index-type="0"><span class="ne-text" style="font-size: 16px">获取瓦片的蜜汁坐标范围</span></li><li id="u0957ab09" data-lake-index-type="0"><span class="ne-text" style="font-size: 16px">转换为经纬度</span></li></ol><pre data-language="javascript" id="k65i9" class="ne-codeblock language-javascript"><code>  pixelsToMeters2(x, y, zoom) {
    let res = this.resolution(zoom);
    let mx = x * res - (2 * Math.PI * 6378137) / 2.0;
    let my = y * res - (2 * Math.PI * 6378137) / 2.0;
    return { x: mx, y: Math.abs(my) };
  }
  tileBounds(x, y, zoom) {
    let minPoints = this.pixelsToMeters2(x * 256, y * 256, zoom);
    let maxPoints = this.pixelsToMeters2((x + 1) * 256, (y + 1) * 256, zoom);
    return {
      x1: minPoints.x,
      x2: maxPoints.x,
      y1: maxPoints.y,
      y2: minPoints.y,
    };
  }
  MetersToLatLon(x, y) {
    let lon = (x / ((2 * Math.PI * 6378137) / 2.0)) * 180.0;
    let lat = (y / ((2 * Math.PI * 6378137) / 2.0)) * 180.0;
    lat =
      (180 / Math.PI) *
      (2 * Math.atan(Math.exp((lat * Math.PI) / 180.0)) - Math.PI / 2.0);
    return { lat: lat, lon: lon };
  }

  tileLatLonBounds(x, y, zoom) {
    let crs = MapContent._crs;
      let envelope = this.tileBounds(x, y, zoom);
      let minLatLon = this.MetersToLatLon(envelope.x1, envelope.y1);
      let maxLatLon = this.MetersToLatLon(envelope.x2, envelope.y2);
      return {
        x1: minLatLon.lon,
        x2: maxLatLon.lon,
        y1: minLatLon.lat,
        y2: maxLatLon.lat,
      };

  }</code></pre><p id="u358ffbb5" class="ne-p"><br></p></details>
**<font style="color:rgb(64, 64, 64);"></font>**



> 更新: 2025-08-14 16:38:48  
> 原文: <https://www.yuque.com/quwaidi-exykz/lv0i4k/ss4ifyrxasxp4g2x>