# 跨系统修改页面导航栏的图标以及标题通过路由传参的心事（hash路由）

:::info
<font style="color:rgba(0, 0, 0, 0.9);">主要用于网页环境中的 URL 处理与页面跳转。例如在数据分析平台中，当用户需要查看特定的数据视图（如全息分析视图）时，通过这段代码可以生成一个携带标题参数（如“数据全息分析”）的 URL，并在新窗口中打开对应的页面，从而为用户提供更丰富的上下文信息和更好的用户体验。</font>

:::

### <font style="color:rgba(0, 0, 0, 0.9);">实现功能</font>
<font style="color:rgba(0, 0, 0, 0.9);">该代码实现了以下功能：</font>

1. **<font style="color:rgba(0, 0, 0, 0.9);">清理 URL</font>**<font style="color:rgba(0, 0, 0, 0.9);">：从输入的字符串中提取出合法的 URL 部分，去除可能的 HTML 标签干扰。</font>
2. **<font style="color:rgba(0, 0, 0, 0.9);">附加查询参数</font>**<font style="color:rgba(0, 0, 0, 0.9);">：将标题参数（如“数据全息分析”）进行 URL 编码后附加到 URL 中，确保参数可以安全传输。</font>
3. **<font style="color:rgba(0, 0, 0, 0.9);">处理哈希部分</font>**<font style="color:rgba(0, 0, 0, 0.9);">：根据 URL 是否包含哈希部分（如</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgba(0, 0, 0, 0.03);">#section</font>`<font style="color:rgba(0, 0, 0, 0.9);">），智能地将查询参数附加到合适的位置。</font>
4. **<font style="color:rgba(0, 0, 0, 0.9);">新窗口打开</font>**<font style="color:rgba(0, 0, 0, 0.9);">：最终在新窗口中打开处理后的 URL，方便用户查看目标页面。</font>

```javascript
  // 清理 URL 字符串，去掉 HTML 标签
        const cleanedUrl = largeScreenUrl.match(/http[s]?:\/\/[^<]+/)[0];
        const titleParam = encodeURIComponent('数据全息分析');
        const url = new URL(cleanedUrl);
        // 检查 URL 是否包含哈希部分
        if (url.hash) {
          // 如果包含哈希部分，将查询参数附加在哈希部分之后
          const hash = url.hash;
          url.hash = ''; // 清除哈希部分
          const newPath = `${url.toString()}${hash}?title=${titleParam}`;
          window.open(newPath, '_blank');
        } else {
          // 如果不包含哈希部分，直接附加查询参数
          url.searchParams.set('title', titleParam);
          const newPath = url.toString();
          window.open(newPath, '_blank');
        }
```

```javascript
const hash = window.location.hash.slice(1); // 去掉哈希路径的 '#' 符号
    const hashParts = hash.split("?");
    const queryParams = hashParts.length > 1 ? hashParts[1] : "";
    const hashParams = new URLSearchParams(queryParams);
    const titleParam = hashParams.get("title");
    if (titleParam) {
      document.title = decodeURIComponent(titleParam);
      // 设置特定的 logo 图片
      const faviconLink = document.querySelector('link[rel="shortcut icon"]');
      if (faviconLink) {
        faviconLink.href = "./logo.png"; // 替换为你的特定 logo 图片路径
      }
    }
```



> 更新: 2025-09-04 17:19:11  
> 原文: <https://www.yuque.com/quwaidi-exykz/lv0i4k/zzi2m7op7gi3aspt>