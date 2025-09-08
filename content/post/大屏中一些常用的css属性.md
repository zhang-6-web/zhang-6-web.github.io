+++
date = '2025-09-08T16:39:21+08:00'
draft = false
title = '大屏中一些常用的css属性'

categories=['css','可视化大屏','鼠标点击事件']

+++

1. <font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(245, 245, 245);">width: fit-content;</font><font style="color:#D22D8D;">设置元素的宽度刚好能够容纳其内容;</font>
2. <font style="color:rgba(0, 0, 0, 0.9);background-color:rgb(245, 245, 245);">box-sizing: border-box;</font><font style="color:#D22D8D;">定义元素的盒模型（box model）的计算方式。它的作用是让元素的宽度和高度包括内容、内边距（padding）和边框（border），但不包括外边距（margin）。这与默认的盒模型（</font>`<font style="color:#D22D8D;background-color:rgba(0, 0, 0, 0.03);">content-box</font>`<font style="color:#D22D8D;">）不同，在默认的盒模型中，宽度和高度只包括内容，不包括内边距和边框。</font>
3. <font style="color:#000000;">aspect-ratio: 4 / 3;</font><font style="color:#D22D8D;">用于保证元素的宽高比例不变；</font>
4. pointer-events;
   - auto:元素会按照正常的文档流顺序来响应鼠标事件。如果元素可见并且没有被其他元素遮挡，那么它就可以成为鼠标事件的目标。
   - none:当设置为 `none` 时，元素不会响应任何鼠标事件，即使鼠标悬停在元素上或点击元素。这相当于元素对于鼠标事件是“透明”的。然而，值得注意的是，`none` 值不会影响元素的子元素。如果子元素没有设置 `pointer-events` 属性，或者显式设置为 `auto`，那么子元素仍然可以响应鼠标事件。
   - visiblePainted：元素只有当它是可见的（即，它有内容，并且没有被 visibility 属性设置为 hidden）并且被绘制到屏幕上时，才会响应鼠标事件。
   - visibleFill：元素只有在它是可见的，并且被绘制为一个填充区域（例如，背景颜色或图片）时，才会响应鼠标事件。
   - visibleStroke：元素只有在它是可见的，并且被绘制为一个轮廓（例如，边框）时，才会响应鼠标事件。
   - visible:元素只有在它是可见的时，才会响应鼠标事件。这包括了 `visiblePainted`、`visibleFill` 和 `visibleStroke` 的所有情况。
   - painted:元素只有在它被绘制到屏幕上时，才会响应鼠标事件，无论它是否可见。
   - fill:元素只有在它被绘制为一个填充区域时，才会响应鼠标事件。
   - stroke:元素只有在它被绘制为一个轮廓时，才会响应鼠标事件。
   - 使用场景：
     1. auto:适用于普通元素，你希望它们能够正常响应鼠标事件。
     2. none:当你希望元素本身不响应鼠标事件，但允许事件穿透到下面的元素时。这在制作覆盖层或者需要鼠标事件穿透的场景中非常有用。
     3. visiblePainted、visibleFill、visibleStroke:这些值通常用于更精细的控制，例如，你可能希望一个元素只有在它有背景颜色时才响应鼠标事件。
