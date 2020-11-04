# echart基础
## options
- title：标题组件，包含主标题和副标题。
- legend：图例中标记的配置，可以通过点击不同的标记控制系列的展示。
- grid：直角坐标系内绘网格图，可用于绘制折线图、柱状图、散点图。
   - xAxis：直角坐标系grid中的x轴。
   - yAxis：直角坐标系的y轴
- polar：极坐标系，可用于散点图和折线图。
   - radiusAxis:极坐标系的径向轴。
   - angleAxis：极坐标系的角度轴。
- radar：雷达图坐标系组件，只适用于雷达图。
- dataZoom：用于区域缩放，支持一下几种组件：
   - dataZoomInside：内置在坐标系中，使用户可以在坐标系上通过鼠标来缩放或者漫游坐标系，使用方法：将type设置为inside。
   - dataZoomSlider：有单独的滚动条，用户在滑动条上进行缩放或漫游，使用方法：将type设置为slider。
   - dataZoomSelect：提供一个选框进行数据区域缩放，现只支持直角坐标系的缩放，需要在toolbox中设置。
- visualMap：视觉映射组件，将数据映射到视觉元素。[详情](https://echarts.apache.org/zh/option.html#visualMap)
- tooltip：提示框组件，可设置在多处，例如：全局、坐标系中(grid.tooltip)、系列中(series.tooltip)以及系列中的每一项(series.data.tooltip)。
- axisPointer：坐标轴指示器的全局公用设置。
- toolbox：工具栏，内置有导出图片，数据视图，动态类型切换，数据区域缩放，重置。
- brush：区域选择组件
- geo：地理坐标系组件，用于地图绘制，支持在地理坐标系上绘制散点图，线集。
- parallel：平行坐标系，通常用于可视化高维数据的图表。
   - parallelAxis：平行坐标系中的坐标轴。
- singleAxis：单轴，用于展示一维数据。
- timeLine：时间线组件，提供在多个ECharts的option间进行切换、播放等的功能。
- graphic：原生图形元素组件，支持的类型有：image, text, circle, sector, ring, polygon, polyline, rect, line, bezierCurve, arc, group。 
- calendar：日历坐标系组件。
- dataset：数据集，使得数据单独管理，可被多个组件复用。
- aria：自动根据图标配置智能生成描述。
- series：通过type设置自己的图表类型：
   - line：折线图
   - bar：柱状图
   - pie：饼图
   - scatter：散点图
   - effectScatter：气泡图
   - radar：雷达图
   - tree：树图，可视化树形数据结构
   - treemap：一种常见的表达层级数据、树状数据的可视化形式，它主要用面积的方式突出树的各级层级中重要的节点。
   - sunburst：旭日图
   - boxplot：箱型图
   - candlestick：k线图
   - heatmap：热力图
   - map：地图
   - parallel：平行坐标系的系列
   - lines：路径图
   - graph：关系图
   - sankey：桑葚图，是一种特殊的流图（可以看作是有向无环图）。 它主要用来表示原材料、能量等如何从最初形式经过中间过程的加工或转化达到最终状态。
   - funnel：漏斗图
   - gauge：仪表盘
   - pictorialBar：象形柱图
   - themeRiver：主题河流，一种特殊的流图。
   - custom：自定义系列
- color：调色盘颜色列表
- backgroundColor：背景色
- textStyle：全局字体样式
- animation：是否开启动画
   - animationThreshold：是否开启动画阀门，当单个系列的图形量大于这个阈值时会关闭动画。
   - animationDuration：初始动画的时长，支持回调函数，可以通过每个数据返回不同的时长实现更戏剧的初始动画效果。
   - animationEasing：初始动画的缓动效果。
   - animationDelay：动画的延迟，支持回调函数。
   - animationDurationUpdate：数据更新动画时长，支持回调。
   - animationEasingUpdate：数据更新动画的缓动效果。
   - animationDelayUpdate：数据更新动画的延迟时长。
- blendMode图形的混合模式。
- hoverLayerThreshold：图形数量阈值，决定是否开启单独的hover层。
- useUTC：是否使用UTC时间。
   
   
   
   
   
   
