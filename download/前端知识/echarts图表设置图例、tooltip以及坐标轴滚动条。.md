# echarts图表设置图例、tooltip以及坐标轴滚动条。

:::info
在配置图表的时候，图表往往都带有图例以及提示框，当图例或提示框中的内容较多时，有时就会对图标造成遮挡，并且也不美观。同样，当x轴坐标刻度值过多时，显示得也会有问题，可以借用echarts自带的一些属性来解决这个问题：

:::

```tsx
// 设置类似于分页一样的效果，不用将图例一次性全部展示出来
legend: {
            type: "scroll",
            orient: "horizontal",
            top: 20,
            data: Array.from(new Set(legend)),
          },
```

```tsx
  tooltip: {
            trigger: "axis",
            enterable: true, // 关键设置
            confine: true, // 加上这一行
            formatter: function (params: any) {
              let html = params
                .map((item: any) => {
                  return `${item.marker}${item.seriesName}: ${item.value}<br/>`;
                })
                .join("");
              return `<div style="max-height:200px;overflow:auto;">${html}</div>`;
            },
          },
```

```tsx
   dataZoom: [
            {
              type: "slider", // 滑动条类型
              show: true, // 显示滑动条
              xAxisIndex: [0], // 指定作用于哪个 xAxis
              filterMode: "filter", // 数据筛选模式
              start: 0, // 初始显示的起始百分比
              end: 50, // 初始显示的结束百分比
              bottom: 10, // 滑动条的位置
              height: 10, // 滑动条的高度
              handleSize: "100%", // 滑动条的把手大小
              handleStyle: {
                color: "#d3dee5",
                shadowBlur: 2,
                shadowColor: "#1e90ff",
                shadowOffsetY: 2,
                shadowOffsetX: 2,
              },
              borderColor: "#90979c",
              borderWidth: 1,
              backgroundColor: "#e2e2e2",
              fillerColor: "rgba(147, 235, 248, 0.2)",
              handleIcon:
                "M-292,0c0,14.8-11.9,26.7-26.7,26.7c-14.8,0-26.7-11.9-26.7-26.7S-306.9-11.9-292-26.7C-292-11.9-280.1,0-292,0z",
              handleIconSize: "100%",
            },
          ],
```



> 更新: 2025-07-16 15:44:15  
> 原文: <https://www.yuque.com/quwaidi-exykz/lv0i4k/le9ci18llzzbc3vz>