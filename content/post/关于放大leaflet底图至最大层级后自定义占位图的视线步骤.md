+++
date = '2025-09-17T16:55:19+08:00'
draft = false
title = '关于放大leaflet底图至最大层级后自定义占位图的视线步骤'

categories=['leaflet','map']

+++

# Leaflet 地图最大层级自定义占位图解决方案

###### 设计思路

1. *监听地图缩放事件，检测是否达到最大层级*
2. *使用瓦片错误处理机制替换无法加载的瓦片*
3. *提供用户友好的提示信息*
4. *添加美观的UI和交互效果*

#### 实现原理

1. 监听瓦片错误事件(就说所有返回的状态码不为200)

2. 将请求失败的瓦片替换成自定义占位图

3. 检测最大缩放层级，自动应用占位图

4. 关键代码：我是在react应用中编写的

   ```js
    // 新增：设置瓦片错误处理
     setupTileErrorHandling = (layer) => {
       if (layer && layer.getLayer) {
         const leafletLayer = layer.getLayer();
         // 设置错误占位图
         if (leafletLayer.options) {
           leafletLayer.options.errorTileUrl = 'https://t1.tianditu.gov.cn/DataServer?T=img_c&x=426805&y=79274&l=19&tk=9d3512a6ad161b05ac68a7fecf05b119';
         }
         // 监听瓦片错误事件
         leafletLayer.on('tileerror', (error) => {
           this.handleTileError(error);
         });
       }
     };
     // 新增：处理瓦片错误
     handleTileError = (error) => {
       const { tile, url } = error;
       // 使用自定义占位图替换失败的瓦片
       if (
         tile &&
         tile.src !==
           'https://t1.tianditu.gov.cn/DataServer?T=img_c&x=426805&y=79274&l=19&tk=9d3512a6ad161b05ac68a7fecf05b119'
       ) {
         tile.src = 'https://t1.tianditu.gov.cn/DataServer?T=img_c&x=426805&y=79274&l=19&tk=9d3512a6ad161b05ac68a7fecf05b119';
       }
     };
   ```

   <img src='/img/oneCode.png'></img>

   至于什么时候调用setupTileErrorHandling这个函数，就是在你每次添加图层到地图上时，就像上图这样

代码示例：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Leaflet 最大层级占位图解决方案</title>
    
    <!-- Leaflet CSS -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            line-height: 1.6;
            color: #333;
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
        }
        
        header {
            text-align: center;
            margin-bottom: 30px;
            padding: 20px;
            background: white;
            border-radius: 10px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }
        
        h1 {
            color: #2c3e50;
            margin-bottom: 10px;
        }
        
        .description {
            color: #7f8c8d;
            max-width: 800px;
            margin: 0 auto;
        }
        
        .map-container {
            display: flex;
            gap: 20px;
            flex-wrap: wrap;
        }
        
        .map-wrapper {
            flex: 1;
            min-width: 300px;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 6px 16px rgba(0, 0, 0, 0.15);
            background: white;
            padding: 15px;
        }
        
        #map {
            height: 500px;
            width: 100%;
            border-radius: 8px;
            z-index: 1;
        }
        
        .instructions {
            flex: 1;
            min-width: 300px;
            background: white;
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 6px 16px rgba(0, 0, 0, 0.15);
        }
        
        .instructions h2 {
            color: #2c3e50;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 2px solid #ecf0f1;
        }
        
        .step {
            margin-bottom: 15px;
            padding: 15px;
            background: #f8f9fa;
            border-radius: 8px;
            border-left: 4px solid #3498db;
        }
        
        .step-number {
            display: inline-block;
            width: 28px;
            height: 28px;
            background: #3498db;
            color: white;
            text-align: center;
            line-height: 28px;
            border-radius: 50%;
            margin-right: 10px;
        }
        
        .code-block {
            background: #2d3436;
            color: #dfe6e9;
            padding: 12px;
            border-radius: 6px;
            overflow-x: auto;
            margin: 15px 0;
            font-family: 'Courier New', monospace;
            font-size: 14px;
        }
        
        .custom-tile {
            display: block;
            margin: 20px auto;
            max-width: 100%;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        
        .notification {
            position: fixed;
            bottom: 20px;
            right: 20px;
            padding: 15px 20px;
            background: #27ae60;
            color: white;
            border-radius: 8px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
            transform: translateY(100px);
            opacity: 0;
            transition: transform 0.3s, opacity 0.3s;
            z-index: 1000;
        }
        
        .notification.show {
            transform: translateY(0);
            opacity: 1;
        }
        
        @media (max-width: 768px) {
            .map-container {
                flex-direction: column;
            }
            
            #map {
                height: 400px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>Leaflet 地图最大层级占位图解决方案</h1>
            <p class="description">
                当地图放大到最大层级时，使用自定义占位图替代空白瓦片，提升用户体验。
            </p>
        </header>
        
        <div class="map-container">
            <div class="map-wrapper">
                <div id="map"></div>
            </div>
                </div>
                <img src="https://t1.tianditu.gov.cn/DataServer?T=img_c&x=426805&y=79274&l=19&tk=9d3512a6ad161b05ac68a7fecf05b119" alt="占位图示例" class="custom-tile">
                <p><strong>提示：</strong> 尝试将地图放大到最大层级，查看占位图效果。</p>
            </div>
        </div>
    </div>
    
    <div class="notification" id="notification">
        已放大到最大层级，显示自定义占位图
    </div>

    <!-- Leaflet JS -->
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    
    <script>
        // 初始化地图
        const map = L.map('map').setView([39.9042, 116.4074], 13); // 北京市中心
        
        // 添加默认图层
        const osmLayer = L.tileLayer('http://10.17.17.243/arcgis/{z}/{y}/{x}.png', {
            attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
            maxZoom: 19
        });
        
        // 添加天地图图层（作为示例）
        const tiandituLayer = L.tileLayer('http://10.17.17.243/arcgis/{z}/{y}/{x}.png', {
            // subdomains: ['0', '1', '2', '3', '4', '5', '6', '7'],
            maxZoom: 22
        });
        
        // 设置瓦片错误处理
        function setupTileErrorHandling(layer) {
            const leafletLayer = layer;
            
            // 设置错误占位图
            if (leafletLayer.options) {
                leafletLayer.options.errorTileUrl = 'https://via.placeholder.com/256x256/3498db/ffffff?text=自定义瓦片';
            }
            
            // 监听瓦片错误事件
            leafletLayer.on('tileerror', function(error) {
                handleTileError(error);
            });
        }

        // 处理瓦片错误
        function handleTileError(error) {
            const { tile } = error;
            
            // 使用自定义占位图替换失败的瓦片
            if (tile) {
                tile.src = 'https://t1.tianditu.gov.cn/DataServer?T=img_c&x=426805&y=79274&l=19&tk=9d3512a6ad161b05ac68a7fecf05b119';
            }
        }
        
        // 添加图层到地图
        osmLayer.addTo(map);
        setupTileErrorHandling(osmLayer);
        
        // 添加图层控制
        const baseMaps = {
            "OpenStreetMap": osmLayer,
            "天地图": tiandituLayer
        };
        
        L.control.layers(baseMaps).addTo(map);
        
        // 监听地图缩放事件
        map.on('zoomend', function() {
            if (map.getZoom() === map.getMaxZoom()) {
                // 显示通知
                const notification = document.getElementById('notification');
                notification.classList.add('show');
                
                // 3秒后隐藏通知
                setTimeout(function() {
                    notification.classList.remove('show');
                }, 3000);
            }
        });
        
        // 添加缩放控件
        L.control.zoom({
            position: 'bottomright'
        }).addTo(map);
        
        // 添加比例尺
        L.control.scale({
            imperial: false,
            position: 'bottomleft'
        }).addTo(map);
    </script>
</body>
</html>
```

上述代码可以直接运行，可以将底图的url换成给自己的，自定义占位符的url也可以自己更换。

效果如下：

<img src='/img/leafletDemo.gif'></img>

