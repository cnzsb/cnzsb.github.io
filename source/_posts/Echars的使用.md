title: 'Echarts 的使用'
date: 2017-10-12 17:10:52
tags: [Echarts, Vue]

---

![echarts-demo](//7xlivs.com1.z0.glb.clouddn.com/2017/10/12/Echarts%E7%9A%84%E4%BD%BF%E7%94%A8/echarts.jpeg)

本文介绍一下 Echarts 的基本配置项，我的项目是在 Vue 中搭配 [vue-echarts](https://github.com/Justineo/vue-echarts) 来使用的。

<!-- more -->

## 引入 Echarts

为了缩减 Echarts 的体积，这里使用了按需引入。

```javascript
import Chart from 'vue-echarts/components/ECharts.vue'
import 'echarts/lib/chart/bar'
import 'echarts/lib/chart/pie'
import 'echarts/lib/component/legend'
import 'echarts/lib/component/tooltip'
import 'echarts/lib/component/grid'
```

在 Vue 中注册 components 后使用：

```html
<chart class="chart" :options="options" auto-resize></chart>
```

## 配置

Echarts 中的配置项基本都提供了相似的 api，例如设置 `title`；`show` 表示是否显示；`text` 表示内容；`textStyle` 表示内容的样式，而 `textStyle` 下会有一些常用的 `css` 对象，如 `color`、`fontSize`、`width` 等。另外还有一些常用的 api，比如在希望格式化某个地方的数据时应该想到 `formatter`；默认的格式一般在 `normal` 中设置，而当获得焦点时的样式在 `emphasis` 中设置。接下来是我所用到的几个配置：

- color 自定义颜色
- tooltip 提示框
    - trigger 确认弹出时机，`item` 饼图，`axis` 柱状图等
    - formatter 格式化数据
- legend 图例说明
    - x 水平定位
    - y 垂直定位
    - data 图例标签，其中的 `name` 对应 `series` 中 `data` 的 `name`
    - formatter
- grid 直角坐标系网格，在使用柱状图等时用来调整视图的位置
    - containLabel 设置 `true` 解决标签自适应宽高的问题
- xAxis 直角坐标系 x 轴，在使用以下属性时，可展开设置内部 `show` 和 相关 `style` 或者 `formatter` 等
    - type 轴类型，`category` 表示类目轴，`value` 表示数值轴
    - axisLine 轴线
    - axisTick 轴线刻度
    - axisLabel 类目标签
    - min 最小值
    - max 最大值
    - splitLine 分割线
- yAxis 直角坐标系 y 轴，同上
- series 系列列表，主要属性
    - type 类型，`pie` 饼图，`bar` 柱状图
    - itemStyle 图形样式
    - label 图例旁边的说明文字
        - normal 默认格式
            - position 位置，`right`、`outside` 等
            - color
            - formatter
    - labelLine 饼图属性，图例与说明文字之间的视觉引导线
        - normal
            - length 第一段引导线长度
            - length2 第二段引导线长度
    - data 数据，可以使用一下属性来标识，也可以使用单一的数组（此时 name 为数组索引）
        - name 数据项名称，即为关键的 `key`，其他与数据联动的地方均依赖此项
        - value 数据值

## 结语

文中提到的属性可满足绝大多数图表的配置了，首次接触 Echarts 的时候可以只关心这几个配置项就足够了。
