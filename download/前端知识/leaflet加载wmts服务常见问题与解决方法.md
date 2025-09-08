# leaflet加载wmts服务常见问题与解决方法

> 问题描述：
>
> 1. 层级不一致问题
>
> 在使用leaflet加载wmts服务时，发现地图的缩放层级（zoom）与wmts服务实际请求的tileMatrix层级不一致，具体表现为：leaflet地图当前zoom为a,但是wmts服务实际请求tileMatrix=a+1,否则无法正确获取瓦片，就会一直报404。
>
> 2. 自定义getTileUrl失效问题
>
> 直接在L.TileLayer.WMTS的options里面传递getTileUrl，发现并不会生效，这是因为leaflet-tileLayer-wmts插件内部会使用自己的getTileUrl实现，不会读取options里面的自定义方法。
>
> 3. tileMatrixCallback兼容性问题
>
> 有些社区版本的leaflet-tileLayer-wmts支持tileMatrixCallBack,但是大多数官方实现并不支持该参数，导致直接传递无效。
>

<details class="lake-collapse"><summary id="u48bc57de"><span class="ne-text">解决方法</span></summary><ol class="ne-ol"><li id="u017975e3" data-lake-index-type="0"><span class="ne-text">通过继承方式重写getTileUrl方法，在方法内部重新设置实际请求的瓦片的层级</span></li></ol><pre data-language="tsx" id="cWvnu" class="ne-codeblock language-tsx"><code>// 1. 继承 L.TileLayer.WMTS 并重写 getTileUrl
var CustomWMTSLayer = L.TileLayer.WMTS.extend({
  getTileUrl: function (coords) {
    var zoom = coords.z;
    var matrixId = zoom + 1; // 让 tileMatrix 比地图 zoom 大1
    // WMTS GetTile URL 模板
    var url =
      &quot;https://36.110.12.227/dmaptiles/tile/service/wmtsi/enhance/tds/1859368250499456/05305fafe7de40b4af9ad80ff6eb3451c&quot; +
      &quot;?service=WMTS&quot; +
      &quot;&amp;request=GetTile&quot; +
      &quot;&amp;version=1.0.0&quot; +
      &quot;&amp;layer=tdt_vec&quot; +
      &quot;&amp;style=default&quot; +
      &quot;&amp;tileMatrixSet=EPSG:4326&quot; +
      &quot;&amp;format=image/png&quot; +
      &quot;&amp;tileMatrix={TileMatrix}&quot; +
      &quot;&amp;tileRow={TileRow}&quot; +
      &quot;&amp;tileCol={TileCol}&quot; +
      &quot;&amp;tk=3e9b06efa6c5413c9127b3f989881c80&quot;;
    // 替换变量
    return url
      .replace(&quot;{TileMatrix}&quot;, matrixId)
      .replace(&quot;{TileRow}&quot;, coords.y)
      .replace(&quot;{TileCol}&quot;, coords.x);
  }
});

// 2. 创建图层
var wmtsLayer = new CustomWMTSLayer(&quot;&quot;, {
  tileSize: 512
});

// 3. 创建地图并添加图层
var map = L.map(&quot;map&quot;, {
  crs: L.CRS.EPSG4326,
  center: [35.505, 100],
  zoom: 4
});
map.addLayer(wmtsLayer);</code></pre></details>


> 更新: 2025-07-18 09:49:13  
> 原文: <https://www.yuque.com/quwaidi-exykz/lv0i4k/bd2ugm9tqknirv8q>