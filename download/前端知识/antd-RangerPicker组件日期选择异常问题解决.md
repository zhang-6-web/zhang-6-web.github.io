# antd-RangerPicker组件日期选择异常问题解决

> **问题描述：**
>
> 在用 RangePicker 选择日期范围时，只选了一个日期（还没选完第二个），日期选择器会自动回到上一次的完整范围，而不是保留你当前的选择。
>

> **详细原因：**
>
> 在设置日期的过程中，将 RangePicker 变成了完全受控组件，它的 value 只能由 startDate 和 endDate 决定。 
>
> 但你的 handleDateChange 只有在选完两个日期后才会 setState（即 dates[0] 和 dates[1] 都有值时才setStartDate/setEndDate），否则 value 还是旧的。 这样，用户只点了一个日期时，value 还是旧的区间，UI 立刻回退。
>

> ******解决方法：**
>
> 用一个 state 专门存储“当前 RangePicker 的临时值” 新增一个 state，比如 rangeValue，类型为 [dayjs | null, dayjs | null]。 
>
> RangePicker 的 value 绑定 rangeValue。 在 onChange 里，无论用户选了一个还是两个日期，都 setRangeValue。 只有当用户选完两个日期时，再 setStartDate/setEndDate，并同步 setFilter。
>





> 更新: 2025-06-27 15:06:58  
> 原文: <https://www.yuque.com/quwaidi-exykz/lv0i4k/wrb9g57ymdg4fpu9>