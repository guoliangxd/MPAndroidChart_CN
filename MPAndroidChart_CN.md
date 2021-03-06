# MPAndroidChart 中文文档

## 1. 入门

*本章介绍了使用这个库的基本设置*

### 添加依赖

首先，将这个库的依赖添加到你的项目中。

**Gradle设置**

```gradle
repositories {
    maven { url 'https://jitpack.io' }
}
dependencies {
    implementation 'com.github.PhilJay:MPAndroidChart:v3.1.0'
}
```

**Maven设置**

```maven
<repository>
    <id>jitpack.io</id>
   <url>https://jitpack.io</url>
</repository>
<!-- <dependencies> section of pom.xml -->
<dependency>
    <groupId>com.github.PhilJay</groupId>
    <artifactId>MPAndroidChart</artifactId>
    <version>v3.1.0</version>
</dependency>
```

推荐使用***Gradle***作为这个库的依赖方式。

### 创建视图

为了使用一个`LineChart`,`BarChart`,`ScatterChart`,`CandleStickChart`,`PieChart`,`BubbleChart`,或`RadarChart`，需要在`.xml`文件中定义：

```xml
<com.github.mikephil.charting.charts.LineChart
        android:id="@+id/chart"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
```

然后在你的`Activity`,`Fragment`或其他位置获取它：

```java
// in this example, a LineChart is initialized from xml
    LineChart chart = (LineChart) findViewById(R.id.chart);
```

或者直接在代码中创建它（之后要把它**加到布局中**）：

```java
    // get a layout defined in xml
    RelativeLayout rl = (RelativeLayout) findViewById(R.id.relativeLayout);
    rl.add(chart); // add the programmatically created chart
```

### 添加数据

在你实例化你的图表后，你可以创建数据并把它添加到图表中。这个例子中使用`LineChart`，它的`Entry`类表示图表中一个具有横纵坐标的单独条目。其他类型的图表，比如`BarChart`，使用其他类（比如`BarEntry`）达到同样的目的。

```java
for (YourData data : dataObjects) {
    // turn your data into Entry objects
    entries.add(new Entry(data.getValueX(), data.getValueY())); 
}
```

下一步，你需要把你创建的`List<Entry>`对象添加到一个`LineDataSet`对象中。`DataSet`对象保存相同归属的对象，并且允许给数据拥设置独立的样式。下面使用的`Label`仅用于描述目的，会在`Legend`显示（要使能该项）。

```java
dataSet.setValueTextColor(...); // styling, ...
```

最后一步，你需要把你创建的那个`LineDataSet`对象（或者多个）添加到一个`LineData`对象中。这个对象保存保存`Chart`实例显示的所有数据，而且允许进一步定制样式。在创建完数据对象后，你需要把它和图表关联，然后刷新图标。

```java
chart.setData(lineData);
chart.invalidate(); // refresh
```

请在基本的设置中参考上述方案。如果想要更多细节上的扩展，请查阅*设置数据*章节，该章基于示例，阐述了如何把数据添加到各种类型的`Chart`中。

### 样式

更多关于图表和数据的设置和样式，请查阅*通用设置与样式*章节。至于各种图表更具体的样式和设置，请查阅*特殊设置与样式*章节。

## 2. 与图表交互

*本库允许您通过图表视图和交互回调方法全面定制可能的触摸（和手势）交互。*

### 启用/禁用交互

* `setTouchEnabled(boolean enabled)`:允许启用/禁用图表所有的触摸交互。
* `setDragEnabled(boolean enabled)`:启用/禁用图表拖拽（平移）。
* `setScaleEnabled(boolean enabled)`:启用/禁用图表横纵坐标的缩放。
* `setScaleXEnabled(boolean enabled)`:启用/禁用图表横坐标缩放。
* `setScaleYEnabled(boolean enabled)`:启用/禁用图表纵坐标缩放。
* `setPinchZoom(boolean enabled)`:如果设置为true，将启用两指缩放。如果禁用，则横纵坐标可以独立缩放。
* `setDoubleTapToZoomEnabled(boolean enabled)`:设置为false的话将禁用双击缩放。 

### 图表抛拽/减速

* `setDragDecelerationEnabled(boolean enabled)`:如果设置为true，图表将在手指离开后持续滚动。默认值：true。
* `setDragDecelerationFrictionCoef(float coef)`:减速摩擦系数的在[0 ; 1]范围内，值越大，减速越慢。例如，如果设置为0，滚动将直接停止。1是无效值，将被自动装换为0.9999。

### 高亮数据

如何通过手势和代码高亮条目请参与*高亮数据*章节。

### 手势回调

`OnChartGestureListener`接口允许您响应对图表的手势操作。

```java public interface OnChartGestureListener {
    /**
     * Callbacks when a touch-gesture has started on the chart (ACTION_DOWN)
     *
     * @param lastPerformedGesture
     */
    void onChartGestureStart(MotionEvent me, ChartGesture lastPerformedGesture);
    /**
     * Callbacks when a touch-gesture has ended on the chart (ACTION_UP, ACTION_CANCEL)
     *
     * @param lastPerformedGesture
     */
    void onChartGestureEnd(MotionEvent me, ChartGesture lastPerformedGesture);
    /**
     * Callbacks when the chart is longpressed.
     */
    public void onChartLongPressed(MotionEvent me);
    /**
     * Callbacks when the chart is double-tapped.
     */
    public void onChartDoubleTapped(MotionEvent me);
    /**
     * Callbacks when the chart is single-tapped.
     */
    public void onChartSingleTapped(MotionEvent me);
    /**
     * Callbacks then a fling gesture is made on the chart.
     * 
     * @param velocityX
     * @param velocityY
     */
    public void onChartFling(MotionEvent me1, MotionEvent me2, float velocityX, float velocityY);
   /**
     * Callbacks when the chart is scaled / zoomed via pinch zoom gesture.
     * 
     * @param scaleX scalefactor on the x-axis
     * @param scaleY scalefactor on the y-axis
     */
    public void onChartScale(MotionEvent me, float scaleX, float scaleY);
   /**
    * Callbacks when the chart is moved / translated via drag gesture.
    *
    * @param dX translation distance on the x-axis
    * @param dY translation distance on the y-axis
    */
    public void onChartTranslate(MotionEvent me, float dX, float dY);
}
```

您只需要让需要接收回调的类实现此接口，并把它作为一个监听器添加到图表。

`chart.setOnChartGestureListener(this);`

## 3. 高亮数据

*本章聚焦于在[v3.0.0](https://github.com/PhilJay/MPAndroidChart/releases)发布的通过手势和程序高亮图表条目的主题*

### 启用/禁用高亮

* `setHighlightPerDragEnabled(boolean enabled)`: 将您的图表此项设为`true`后，当图表最小化时，每次拖动图表都会高亮。默认值：`true`。

* `setHighlightPerTapEnabled(boolean enabled)`:将您的图表此项设为`false`将禁用通过点击手势高亮数据。但数据仍可通过高亮或程序来高亮。默认值：`true`。

* `setMaxHighlightDistance(float distanceDp)`:以`dp`为单位，设置最大高亮距离。在远于某条目此距离的位置点击不会触发高亮。默认值：`500dp`。

此外，可以为单个数据集对象配置高亮：

```java
  dataSet.setHighlightEnabled(true); // allow highlighting for DataSet
  // set this to false to disable the drawing of highlight indicator (lines)
  dataSet.setDrawHighlightIndicators(true); 
  dataSet.setHighlightColor(Color.BLACK); // color for highlight indicator
  // and more...
```

### 通过程序高亮

* `highlightValue(float x, int dataSetIndex, boolean callListener)`:根据传入的横坐标，高亮已有数据集中相应位置的数据。`dataSetIndex`传入-1将取消所有高亮。布尔标志决定是否会调用选择的监听器。
* `highlightValue(Highlight high, boolean callListener)`:高亮`2Highlight`对象代表的数据。传入`null`将取消所有高亮。布尔标志决定是否会调用选择的监听器。
* `highlightValues(Highlight[] highs)`:高亮传入的`Highlight[]`数组所代表的数据。传入`null`或者空数组将取消所有高亮。
* `getHighlighted()`:返回包含所有高亮条目横坐标索引，数据集索引等信息的`Highlight[]`数据。

### 选择回调

这个库提供了大量交互回调的监听器。其中之一是通过触摸高亮数据时用来回调的`OnChartValueSelectedListener`：

```java
public interface OnChartValueSelectedListener {
    /**
    * Called when a value has been selected inside the chart.
    *
    * @param e The selected Entry.
    * @param h The corresponding highlight object that contains information
    * about the highlighted position
    */
    public void onValueSelected(Entry e, Highlight h);
    /**
    * Called when nothing has been selected or an "un-select" has been made.
    */
    public void onNothingSelected();

```

您只需要让需要接收回调的类实现此接口，并把它作为一个监听器添加到图表。

```java
chart.setOnChartValueSelectedListener(this);
```

### Highlight类

`Highlight`类表示与一个高亮条目相关的数据，比如比如高亮条目对象本身，它所归属的数据集，它在图层绘制中的位置等等。它能用来获取已经高亮的元素的条目的信息，或者给图表提供将要被高亮的条目的信息。关于这个目的，`Highlight`类提供两个构造函数：

```java
// constructor for standard highlight
public Highlight(float x, int dataSetIndex) { ... }
// constructor for stacked BarEntry highlight
public Highlight(float x, int dataSetIndex, int stackIndex) { ... }
```

这些构造函数可以用来创建用于执行通过程序高亮的`Highlight`类：

```java
// highlight the entry and x-position 50 in the first (0) DataSet
Highlight highlight = new Highlight(50f, 0); 
chart.highlightValue(highlight, false); // highlight this value, don't call listener
```

### 定制高亮器

所有用户输入都会以高亮手势的形式在内部通过默认的`ChartHighlighter`类来处理。可以通过以下方法让用户实现来代替默认的高亮器。

* `setHighlighter(ChartHighlighter highlighter)`:给图表设置一个用户定义的能处理所有图表视图上高亮手势的对象。您定义的高亮器对象需要继承`Charthighlighter`类。

## 4. 坐标轴

*本章聚焦于`AxisBase`类，它是`XAxis`,`YAxis`类的基类。在v2.0.0引入。*

### AxisBase

下面提到的方法可以**应用于横纵坐标轴**。

坐标轴类可以用来定制化样式，包含以下组件/部分：

* 刻度（在竖直（y轴）或水平（x轴）方向对齐绘制），包含描述数据的坐标值。
* 轴线，紧邻并平行于刻度绘制。
* 网格线，在水平方向上源于刻度绘制。
* 限制线，用于显示边界或约束之类的特殊信息。

### 控制坐标轴绘制部分

* `setEnabled(boolean enabled)`: 启用/禁用坐标轴。如果禁用，无论其他项如何设置，坐标轴的任何部分都不会被绘制。
* `setDrawLabels(boolean enabled)`:设置true启用绘制坐标轴刻度。 
* `setDrawAxisLine(boolean enabled)`: 启用/禁用轴线。
* `setDrawGridLines(boolean enabled)`: 启用/禁用网格线。

### 定制坐标轴的范围（最大值/最小值）

* `setAxisMaximum(float max)`:给坐标轴设置一个用户最大值。如果设置了，此值是否被计算取决于提供的数据。
* `resetAxisMaximum()`: 调用此方法将撤销之前设置最大值的操作。通过这个方法，您可以再次允许坐标轴自动计算它的最大值。
* `setAxisMinimum(float min)`:给最标轴设置一个用户最小值。如果设置了，此值是否被计算取决于提供的数据。
* `resetAxisMinimum()`: 调用此方法将撤销之前设置最小值的操作。通过这个方法，您可以再次允许坐标轴自动计算它的最小值。
* `setStartAtZero(boolean enabled)`: *弃用*—使用`setAxisMinValue(...)`或`setAxisMaxValue(...)`代替。
* `setInverted(boolean enabled)`:如果设置为true，坐标轴将反转，这意味着最大值将在底部，最小值将在顶部。
* `setSpaceTop(float percent)`:设置图表中最高值相比轴上最高值的间距（在整个坐标轴范围内的百分比）。
* `setSpaceBottom(float percent)`:设置图表中最低值相比轴上最低值的间距（在整个坐标轴范围内的百分比）。
* `setShowOnlyMinMax(boolean enabled)`: 如果启用，坐标轴将只显示它的最大值和最小值。这将忽略/覆盖定义的刻度数（如果不是强制的）。
* `setLabelCount(int count, boolean force)`:设置y轴的刻度数。要知道这并不是固定的（if force == false），可能只是一个近似值。如果启用了force(true)，将会绘制特定数量的刻度，这可能导致坐标轴不均匀。
* `setPosition(YAxisLabelPosition pos)`:设置坐标轴刻度绘制的位置。要么`INSIDE_CHART`,要么`OUTSIDE_CHART`。
* `setGranularity(float gran)`:设置y周两个值之间的最小间距。这可以避免缩放导致的坐标重叠。
* `setGranularityEnabled(boolean enabled)`: 启用粒度特性将在缩放时限制y轴间距。默认值：false。

### 修改坐标轴样式

* `setTextColor(int color)`: 设置坐标轴刻度颜色。
* `setTextSize(float size)`: 以`dp`为单位设置坐标轴刻度字号。
* `setTypeface(Typeface tf)`:给坐标轴刻度设置一个用户`Typeface`。
* `setGridColor(int color)`:设置坐标轴网格线颜色。
* `setGridLineWidth(float width)`: 设置坐标轴网格线宽度。
* `setAxisLineColor(int color)`:设置轴线颜色。 
* `setAxisLineWidth(float width)`:设置轴线宽度。
* `enableGridDashedLine(float lineLength, float spaceLength, float phase)`: 启用以虚线方式绘制网格线，例如"- - - - - -"。"lineLength"控制每条线段的长度，"spaceLength"控制线段的间距，"phase"控制起始位置。

### 格式化坐标轴数据

为了格式化坐标数据，您可以使用`ValueFormatter`类。详情请查阅*格式化数据*章节。

### 限制线

横纵坐标轴都支持用限制线来显示像边界和约束这样的特殊信息。限制线添加到纵轴会水平绘制，添加到横轴会竖直绘制。下面是给坐标轴添加/移除限制线的方法：

* `addLimitLine(LimitLine l)`:给这个坐标轴添加一条新限制线。
* `removeLimitLine(LimitLine l)`:从这个坐标轴移除某条特定的限制线。
*  还有更多用于添加/移除的方法。
* `setDrawLimitLinesBehindData(boolean enabled)`:用于控制限制线和真是数据的层叠关系。如果设置为true，限制线将在真实数据之下绘制，否之在上面。默认值：false。

限制线（`LimitLine`类）是（正如名字所暗示的）用来给用户提供额外信息的线。

举个例子，您的表格要显示用户通过APP记录的大量血压测量结果。为了通知用户高于140mmHg的收缩压可能涉及健康风险，您可以通过添加一条140的限制线来提供这个信息。

### 代码示例

```java
YAxis leftAxis = chart.getAxisLeft();
LimitLine ll = new LimitLine(140f, "Blood Pressure High");
ll.setLineColor(Color.RED);
ll.setLineWidth(4f);
ll.setTextColor(Color.BLACK);
ll.setTextSize(12f);
// .. and more styling options
leftAxis.addLimitLine(ll);
```

## 5. 横坐标

### XAxis

`XAxis`是`AxisBase`的子类，继承了其大量样式和常用方法。

`XAxis`类（在之前的2.0.0版本中称为`XLabels`）是和水平坐标轴相关的一切信息和数据的容器。每个`LineChart`,`BarChart`,`ScatterChart`,`CandleStick`和`RadarChart`都有一个`XAxis`对象。

`XAxis`类允许特殊的样式，包含如下组件/部分：

* 紧邻并且平行于刻度绘制的轴线。
* 在竖直方向源于坐标轴刻度的网格线。

```java
XAxis xAxis = chart.getXAxis();
```

### 定制坐标轴数据

* `setLabelRotationAngle(float angle)`: 设置绘制横坐标刻度的角度（以度为单位）。
* `setPosition(XAxisPosition pos)`:设置横坐标出现的位置。可以在`TOP`,`BOTTOM`,`BOTH_SIDEN`,`TOP_INSIDE`或`BOTTOM_INSIDE`之间选择。

### 代码示例

```java
XAxis xAxis = chart.getXAxis();
xAxis.setPosition(XAxisPosition.BOTTOM);
xAxis.setTextSize(10f);
xAxis.setTextColor(Color.RED);
xAxis.setDrawAxisLine(true);
xAxis.setDrawGridLines(false);
// set a custom value formatter
xAxis.setValueFormatter(new MyCustomFormatter()); 
// and more...
```

## 6. 纵坐标

### YAxis

`YAxis`是`AxisBase`类的子类。本条目只描述`YAxis`，不会描述它的父类。

`YAxis`类（在2.0.0版本之前称为`YLabels`）是关于竖直坐标所有信息和数据的容器。每个`LineChart`,`BarChart`,`ScatterChart`,`CandleStick`都有一对相互独立的左`YAxis`和右`YAxis`对象。`RadarChart`只有一个`YAxis`。默认两个坐标轴都会被绘制。

可通过下面任意方法**获取`YAxis`类的实例**。

```java
YAxis leftAxis = chart.getAxisLeft();
YAxis rightAxis = chart.getAxisRight();
YAxis leftAxis = chart.getAxis(AxisDependency.LEFT);
YAxis yAxis = radarChart.getYAxis(); // this method radarchart only
```

在运行时，使用`public AxisDependency getAxisDependency()`来决定显示哪边的纵轴。

### 坐标轴依赖

默认情况下，所有的数据都被添加到图表左纵坐标轴的布局中。如果不进一步指定和启动，右纵坐标轴会调整适应左坐标轴的缩放。

如果您需要支持不同的坐标轴缩放，您可以通过设置您的数据将要对应那个坐标轴来绘制来实现。这个可以通过修改您的`DataSet`对象的`AxisDependency`来实现。

```java
LineDataSet dataSet = ...; // get a dataset
dataSet.setAxisDependency(AxisDependency.RIGHT);
```

这样设置将会改变您数据绘制的坐标轴。

### 零线

水平方向沿源自纵轴每个值的线除了网格线外，还有零线。零线在坐标轴零值的位置绘制，它和网格线相似，但可以单独配置。

* `setDrawZeroLine(boolean enabled)`:启用/禁用零线绘制。
* `setZeroLineWidth(float width)`:设置零线宽度。
* `setZeroLineColor(int color)`: 设置零线颜色。

零线示例代码：

```java
// data has AxisDependency.LEFT
YAxis left = mChart.getAxisLeft();
left.setDrawLabels(false); // no axis labels
left.setDrawAxisLine(false); // no axis line
left.setDrawGridLines(false); // no grid lines
left.setDrawZeroLine(true); // draw a zero line
mChart.getAxisRight().setEnabled(false); // no right axis
```

上述代码将会产生一个没有刻度，没有网格线或者轴线，只有零线的表格。

### 更多代码示例

```java
YAxis yAxis = mChart.getAxisLeft();
yAxis.setTypeface(...); // set a different font
yAxis.setTextSize(12f); // set the text size
yAxis.setAxisMinimum(0f); // start at zero
yAxis.setAxisMaximum(100f); // the axis maximum is 100
yAxis.setTextColor(Color.BLACK);
yAxis.setValueFormatter(new MyValueFormatter());
yAxis.setGranularity(1f); // interval 1
yAxis.setLabelCount(6, true); // force 6 labels
//... and more
```

## 7. 设置数据

*本章涵盖了为各种图表设置数据的主题*

### LineChart（折线图）

如果您想给图表添加数据，可以调用如下方法：

```java
public void setData(ChartData data) { ... }
```

基类`ChartData`封装了图表渲染时所需的所有数据和信息。对于每种类型的图表，都有一个用于为图表设置数据的`ChartData`的子类（例如`LineChart`）。在构造函数中，您可以传入一个`List<? extends IDataSet>`作为将要显示的数据。下面的例子展示了用于给`LineChart`添加数据的`LineData`类（继承了`ChartData`）的构造函数。

```java
    /** List constructor */
    public LineData(List<ILineDataSet> sets) { ... }
    /** Constructor with one or multiple ILineDataSet objects */
    public LineData(ILineDataSet...) { ... }
```

那么，什么时`DataSet`，为什么您需要它？这很简单。一个`DataSet`对象表示了一组在图表中有相同归属的条目（例如`Entry`类）。它被设计用来在逻辑上区分图表中不同组的数据。对于每种类型的图表，都有一个允许定制样式的`DataSet`的子类（例如`LineDataSet`）。

举个例子，你可能想在一个`LineChart`中显示两个公司一年内的大量营业额。在这种情况下，推荐创建两个不同的`LineDataSet`对象，每个对象包含4个数据（一个数据代表一个季度）。

当然，也可以只用一个`LineDataSet`对象包含两个公司的8个数据。

那么，怎样建立一个`LineDataSet`对象？

```java
public LineDataSet(List<Entry> entries, String label) { ... }
```

当查看构造函数时（可能有多个构造函数），可以看到`LineDataSet`需要一个类型为`Entry`的`List`还有一个用来描述`LineDataSet`并且作为`Legend`标签的字符串。进一步，这个标签可以用来在`LineData`中的其他`LineDataSet`中找到这个`LineDataSet`.

`Entry`类型的`List`封装了图表所有的数据。一个`Entry`对象是对图表中横纵坐标值额外的封装。

```java
   public Entry(float x, float y) { ... }
```

把数据组合到一起（以两个公司一年季度营业额为例）：

首先，创建一个`Entry`类型的列表用来保存数据：

```java
 	List<Entry> valsComp1 = new ArrayList<Entry>();
    List<Entry> valsComp2 = new ArrayList<Entry>();
```

然后，用`Entry`对象填充列表。确保条目对象有正确横轴索引。（当让，这里也可以使用循环。在那种情况下，循环计数变量可以作为横轴索引）。

```java
    Entry c1e1 = new Entry(0f, 100000f); // 0 == quarter 1
    valsComp1.add(c1e1);
    Entry c1e2 = new Entry(1f, 140000f); // 1 == quarter 2 ...
    valsComp1.add(c1e2);
    // and so on ...
    Entry c2e1 = new Entry(0f, 130000f); // 0 == quarter 1
    valsComp2.add(c2e1);
    Entry c2e2 = new Entry(1f, 115000f); // 1 == quarter 2 ...
    valsComp2.add(c2e2);
    //..
```

既然我们应有了`Entry`对象的列表，现在可以创建`LineDataSet`对象了：

```java
    LineDataSet setComp1 = new LineDataSet(valsComp1, "Company 1");
    setComp1.setAxisDependency(AxisDependency.LEFT);
    LineDataSet setComp2 = new LineDataSet(valsComp2, "Company 2");
    setComp2.setAxisDependency(AxisDependency.LEFT);
```

通过调用`setAxisDependency(...)`,`DataSet`可以指定对应哪边的纵轴来绘制。

最后但不是最不重要的一点，我们创建一个`IDataSets`列表然后构建我们的`CharData`对象：

```java
    // use the interface ILineDataSet
    List<ILineDataSet> dataSets = new ArrayList<ILineDataSet>();
    dataSets.add(setComp1);
    dataSets.add(setComp2);
    LineData data = new LineData(dataSets);
    mLineChart.setData(data);
    mLineChart.invalidate(); // refresh
```

在调用`invalidate()`后，图表将被刷新，提供的数据将被绘制。

如果我们想要在横坐标上加更多描述性的数据（而不是用数字0到3表示4个季度）,我们可以通过使用`ValueFormatter`类来完成这种操作。这个类允许在横轴（还有更多地方）绘制自定义样式的值。在这个例子中，是按如下方式格式化数据的：

```java
// the labels that should be drawn on the XAxis
final String[] quarters = new String[] { "Q1", "Q2", "Q3", "Q4" };
ValueFormatter formatter = new ValueFormatter() {
    @Override
    public String getAxisLabel(float value, AxisBase axis) {
        return quarters[(int) value];
    }
};
XAxis xAxis = mLineChart.getXAxis();
xAxis.setGranularity(1f); // minimum axis-step (interval) is 1
xAxis.setValueFormatter(formatter);
```

格式化数的更多细节请查阅*格式化数据*章节。

给普通的`BarChart`，`ScatterChart`，`BubbleChart``CandleStickChart`设置数据和给`LineChart`类似。一个特殊的案例是带有多个柱形（分组）的`BarChart`，后面将解释这种案例。

#### 条目的顺序

请意识到本库官方并不支持用一组横坐未按升序排序的条目绘制`LineChart`数据。不按顺序添加条目也许能正常绘制图表，但也可能导致一项不到的表现。一个`Entry`列表对象可以手工排序或者借住`EntryXComparator`：

```java
List<Entry> entries = ...;
Collections.sort(entries, new EntryXComparator());
```

为什么需要这样的理由是由于本库使用了二分查找算法，只有在有序的列表上才会有更好的性能表现。

### BarChart（柱状图）

`BarChart`设置数据的方式与`LineChart`非常相似。主要的差异在设置给图表的数据对象上（例如`BarEntry`替代了`Entry`）。除此之外，`BarChart`还有几点样式上的差别。

考虑以下用数据填充`BarChart`的示例：

```java
List<BarEntry> entries = new ArrayList<>();
entries.add(new BarEntry(0f, 30f));
entries.add(new BarEntry(1f, 80f));
entries.add(new BarEntry(2f, 60f));
entries.add(new BarEntry(3f, 50f)); 
                                    // gap of 2f
entries.add(new BarEntry(5f, 70f));
entries.add(new BarEntry(6f, 60f));
BarDataSet set = new BarDataSet(entries, "BarDataSet");
```

在上面的示例中，创建了5个`BarEntry`对象并将它们添加到了一个`BarDataSet`中。注意在第4和第5个条目之间有“2”的间隙。在这个例子中，这个间隙用来展示`BarChart`如何在图表中定位柱形。下一步，要创建一个`BarData`对象：

```java
BarData data = new BarData(set);
data.setBarWidth(0.9f); // set custom bar width
chart.setData(data);
chart.setFitBars(true); // make the x-axis fit exactly all bars
chart.invalidate(); // refresh
```

在上面的代码片段中，创建了一个`BarData`对象。当为图表创建`BarEntry`对象时，我们为每个柱形在横轴上预留了"1f"的空间（以横坐标为中心）。通过设置柱形的宽度为0.9f，我们创建了0.1f的柱形间距。调用`setFitBars(true)`将通知图表根据给定的数据自动调整横轴坐标范围，不会有柱形在边缘被截断。

在创建`BarData`对象后，我们把它设置给图表并刷新。

### Grouped BarChart（分组柱状图）

从v3.0.0发布版开始，MPAndroidChart支持以显示分组或用户自定义的方式绘制柱形（在这种情况下本库将处理横坐标）。这意味着用户可以把柱形放在他想要的任意位置。

这一节主要关注显示分组的`BarChart`，这意味着本库将处理柱形的横坐标。

考虑如下示例：

```java
YourData[] group1 = ...;
YourData[] group2 = ...;
List<BarEntry> entriesGroup1 = new ArrayList<>();
List<BarEntry> entriesGroup2 = new ArrayList<>();
// fill the lists
for(int i = 0; i < group1.length; i++) {
    entriesGroup1.add(new BarEntry(i, group1.getValue()));
    entriesGroup2.add(new BarEntry(i, group2.getValue()));
}
BarDataSet set1 = new BarDataSet(entriesGroup1, "Group 1");
BarDataSet set2 = new BarDataSet(entriesGroup2, "Group 2");
```

在这个例子中，我们有两组柱形，每组均由一个独立的`BarDataSet`表示。在显式分组中（由库函数处理），条目确切的横坐标并不重要。分组是基于条目列表中`BarEntry`的位置执行的。

```java
float groupSpace = 0.06f;
float barSpace = 0.02f; // x2 dataset
float barWidth = 0.45f; // x2 dataset
// (0.02 + 0.45) * 2 + 0.06 = 1.00 -> interval per "group"
BarData data = new BarData(set1, set2);
data.setBarWidth(barWidth); // set the width of each bar
barChart.setData(data);
barChart.groupBars(1980f, groupSpace, barSpace); // perform the "explicit" grouping
barChart.invalidate(); // refresh
```

在上面的代码片段中，`BarDataSet`对象被添加到了`BarChart`中。`groupBars(...)`方法执行两个`BarDataSet`的分组。这个方法需要如下的参数：

```java
public void groupBars(float fromX, float groupSpace, float barSpace) { ... }
```

参数`fromX`决定了已分组的柱形从哪里开始（在这个例子中"1980"），`groupSpace`决定了组间距，`barSpace`决定了组内柱形的间距。基于这些参数，`groupBars(...)`函数将横轴上每个柱形的位置调整为分组外观，并保留单个`BarEntry`对象的顺序。

每个组在横轴上的”间隔“（占用空间）也由参数`groupSpace`,`barSpace`,`barWidth`决定。

当然，也可以不用`groupBar(...)`方法来实现分组柱状图，只需要手动在横轴上定位各个柱形的位置。

为了使横轴的刻度在每组的中央，您可以使用`setCenterAxisLabels(...)`方法：

```java
XAxis xAxis = chart.getXAxis();
xAxis.setCenterAxisLabels(true);
```

### Stacked BarChart（堆栈柱状图）

堆叠柱状图的建立和普通柱状图非常相似，但创建单个`BarEntry`的方式除外。如果是堆叠的柱形图，则要使用不同的`BarEntry`构造函数：

```java
public BarEntry(float x, float [] values) { ... }
```

这个构造函数允许给纵坐标提供多个值（通过数组），这些值表示每个柱形的"堆栈"值，数组中的每个值都是堆栈的一部分。

考虑如下示例：

```java
BarEntry stackedEntry = new BarEntry(0f, new float[] { 10, 20, 30 });
```

这个`BarEntry`的横轴位置为0f，纵轴值由三部分组成，累加的高度为60（10 + 20 + 30）。

### PieChart（饼状图）

与其他类型的图表不同，`PieChart`以`PieEntry`对象的方式回去数据。这些对象的构造函数如下所示：

```java
public PieEntry(float value, String label) { ... }
```

构造函数的第一个参数是在饼状图中用于绘制切片的实际值，第二个字符串参数被称为标签，用于提供描述切片的额外信息。考虑如下`PieChart`的设置示例：

```java
List<PieEntry> entries = new ArrayList<>();
entries.add(new PieEntry(18.5f, "Green"));
entries.add(new PieEntry(26.7f, "Yellow"));
entries.add(new PieEntry(24.0f, "Red"));
entries.add(new PieEntry(30.8f, "Blue"));
PieDataSet set = new PieDataSet(entries, "Election Results");
PieData data = new PieData(set);
pieChart.setData(data);
pieChart.invalidate(); // refresh
```

`PieEntry`对象无需保存横坐标值，因为`PieEntry`对象显示的顺序由条目列表中的顺序决定。

## 8. 设置颜色

*从v1.4.0发布版本开始，不再需要之前发布版中用于设置颜色的`ColorTemplate`对象（弃用）。*尽管如此，它依然保存所有预定义的颜色数组（例如`ColorTemplate.VORDIPLOM_COLORS`），并提供常用的将资源（资源整数）转换为真正颜色的方法。

代替`ColorTemplate`，颜色现在可以直接通过`DataSet`对象指定，而且允许每个`DataSet`独立定义样式。

在这个简短的例子中，我们有两组代表两家公司季度营业额的`LineDataSet`对象（在之前的*设置数据*章节提到过）。我们现在想给它们设置不同的颜色。

我们想要的是：

* "Company1"的数据由四种有差异的红色来表示
* "Company2"的数据由四种有差异的绿色来表示

我们打代码看起来像这样：

```java
LineDataSet setComp1 = new LineDataSet(valsComp1, "Company 1");
  // sets colors for the dataset, resolution of the resource name to a "real" color is done internally
  setComp1.setColors(new int[] { R.color.red1, R.color.red2, R.color.red3, R.color.red4 }, Context);
  LineDataSet setComp2 = new LineDataSet(valsComp2, "Company 2");
  setComp2.setColors(new int[] { R.color.green1, R.color.green2, R.color.green3, R.color.green4 }, Context);
```

除此之外，还有很多方法用来给`DataSet`设置颜色。以下是全部说明：

* `setColors(int [] colors, Context c)`:设置`DataSet`前部使用的颜色。当`DataSet`代表的条目数多余颜色数组时，颜色会被复用。您可以使用`new int[]{R.color.red, R.color.green,...}`来给这个方法提供颜色。颜色在内部会被解析。
* `setColors(int [] colors)`:设置`DataSet`前部使用的颜色。当`DataSet`代表的条目数多余颜色数组时，颜色会被复用。确保在把颜色添加给`DataSet`前已经准备好（通过调用`getResources().getColor(...)`）。
* `setColors(ArrayList colors)`: 设置`DataSet`前部使用的颜色。当`DataSet`代表的条目数多余颜色数组时，颜色会被复用。确保在把颜色添加给`DataSet`前已经准备好（通过调用`getResources().getColor(...)`）
* `setColor(int color)`: 设置给`DataSet`使用的唯一颜色。在内部会创建颜色数组并添加该颜色。

`ColorTemplate`示例

```java
LineDataSet set = new LineDataSet(...);
set.setColors(ColorTemplate.VORDIPLOM_COLORS);
```

如果没有给`DataSet`设置颜色，将使用默认的颜色。

## 9. 格式化数据(ValueFormatter)

*于v1.6.2添加—于v2.1.4改进*

### ValueFormatter

`ValueFormatter`类可用来创建允许在绘制图表内数据（来自`DataSet`或者坐标轴）前，以特定方式格式化它们的自定义格式化器类。

为了使用`ValueFormatter`类，您可以简单的创建一个继承了它类，然后重写你需要的方法。`ValueFormatter`提供了不同的方法用于格式化来自不同类型图表和坐标轴的数据。

如果没有方法被重写，`ValueFormatter`将返回默认的模式，通常是一个字符串代表条目的纵坐标（或者坐标轴刻度）。

### 创建一个ValueFormatter（图表和纵坐标）

下面个格式化器给`LineChart`，`BarChart`条目还有坐标轴(纵轴）数据提供了定制化的格式化：

```java
class MyValueFormatter : ValueFormatter() {
    private val format = DecimalFormat("###,##0.0")
    // override this for e.g. LineChart or ScatterChart
    override fun getPointLabel(entry: Entry?): String {
        return format.format(entry?.y)
    }
    // override this for BarChart
    override fun getBarLabel(barEntry: BarEntry?): String {
        return format.format(barEntry?.y)
    }
    // override this for custom formatting of XAxis or YAxis labels
    override fun getAxisLabel(value: Float, axis: AxisBase?): String {
        return format.format(value)
    }
    // ... override other methods for the other chart types
}
```



### 创建一个ValueFormatter（横坐标）

下面的格式化器设计用来将横坐标数据格式化为一周七天。注意坐标轴索引要安全转换成整数并用作数组索引。同样，您需要确保数组的长度于图表横轴显示的数据范围一致。

```java
class MyXAxisFormatter : ValueFormatter() {
    private val days = arrayOf("Mo", "Tu", "Wed", "Th", "Fr", "Sa", "Su")
    override fun getAxisLabel(value: Float, axis: AxisBase?): String {
        return days.getOrNull(value.toInt()) ?: value.toString()
    }
}
```

然后，为了格式化数据，将您的格式化器设置给`ChartData`或`DataSet`对象。

```java
// usage on whole data object (e.g. LineData)
lineData.setValueFormatter(MyValueFormatter())
// usage on individual dataset object (e.g. LineDataSet)
lineDataSet.valueFormatter = MyValueFormatter()
```

或者，为了格式化坐标轴数据，将您的格式化器设置给`XAxis`或者`YAxis`对象。

```java
// XAxis
chart.xAxis.valueFormatter = MyXAxisFormatter()
// left YAxis
chart.leftAxis.valueFormatter = MyValueFormatter()
```

### 预定义格式化器

* [LargeValueFormatter](https://github.com/PhilJay/MPAndroidChart/blob/master/MPChartLib/src/main/java/com/github/mikephil/charting/formatter/LargeValueFormatter.java):用于格式化大于1000的值。它将把"1.000"转换为"1,"1.000.000”转换为"1m"，“1.000.000.000”转换为“1b”，“1.000.000.000.000”转换为“1T”。
* [PercentFormatter](https://github.com/PhilJay/MPAndroidChart/blob/master/MPChartLib/src/main/java/com/github/mikephil/charting/formatter/PercentFormatter.java):用来在一个十进制数后显示“%”在`PieChart`中很有用。50->50.0%。
* [StackedValueFormatter](https://github.com/PhilJay/MPAndroidChart/blob/master/MPChartLib/src/main/java/com/github/mikephil/charting/formatter/StackedValueFormatter.java):一种专门为堆叠`BarChart`设计的格式化器。它允许定义绘制所有堆叠数据还是只绘制顶层数据。

### 注意事项

从v3.1.0发布版开始，坐标数据和图表数据，它们现在都有`ValueFormatter`格式化。

## 10. 通用设置与样式

*这章聚焦于能应用于所有图表上的设置和样式*

### 刷新

* `invalidate()`: 调用图表的该方法将刷新（重新绘制）图表。在需要图表上生效的变更展现出来时调用。
* `notifyDataSetChanged()`: 通知图表其基本数据已经改变，并且要执行所有必要的重新计算（偏移，图例，最大值，最小值...）。需要在动态添加数据后调用。

### 日志

* `setLogEnabled(boolean enabled)`:设置为true将激活图表日志输出。启用后会影响性能，除非必要，请保持禁用。

### 通用图表设置

下面是一些您能直接在图表上调用的通用样式方法：

* `setBackgroundColor(int color)`: 设置将覆盖整个图表视图的背景颜色。另外，背景颜色也能在`.xml`类型的布局文件中设置。
* `setDescription(String desc)`: 设置出现在图表右下角的描述文本。
* `setDescriptionColor(int color)`:设置描述文本的颜色。 
* `setDescriptionPosition(float x, float y)`:以像素为单位，设置描述文本在屏幕上的指定位置。
* `setDescriptionTypeface(Typeface t)`:设置绘制描述文本使用的 `Typeface`。
* `setDescriptionTextSize(float size)`:以像素为单位，设置描述文本的字号，最小6f，最大16f。
* `setNoDataText(String text)`:设置在图表为空时显示的文本。
* `setDrawGridBackground(boolean enabled)`:如果启用，将绘制图表区域下层的矩形背景。
* `setGridBackgroundColor(int color)`:设置绘制网格背景的颜色。
* `setDrawBorders(boolean enabled)`:启用/禁用绘制图表边界（环绕图表的线）。
* `setBorderColor(int color)`:设置图表边界线的颜色。
* `setBorderWidth(float width)`: 以dp为单位，设置图表边界的宽度。
* `setMaxVisibleValueCount(int count)`:设置图表最大可见刻度数量。只在`setDrawValues()`启用时生效。

## 11. 特殊设置与样式

在*入门*和*通用设置与样式*章节介绍了所有图表通用的的设置和样式方法。*本章聚焦于每种图表特有的设置*。

### Line-,Bar-,Scatter-,Candle-与BubbleChart

* `setAutoScaleMinMaxEnabled(boolean enabled)`: 是否启用自动缩放纵轴。如果启用，当视点改变时，纵轴将自动根据当前横轴范围内纵坐标的最大值和最小值进行缩放。在显示金融数据时很有用。默认值：false。
* `setKeepPositionOnRotation(boolean enabled)`: 设置在屏幕方向改变后，图表是否会保持原来的位置（在缩放和滚动方面）。默认值：false。

### BarChart

* `setDrawValueAboveBar(boolean enabled)`: 如果设置为true，所有的数据都将绘制在柱形上方，而不是在柱形顶端下面。
* `setDrawBarShadow(boolean enabled)`: 如果设置为true，在每个柱形下层将绘制一个表示最大值的灰色区域。启用该项将降低大约40%的性能。
* `setDrawValuesForWholeStack(boolean enabled)`:如果设置为true，所有堆叠柱形的数据都将单独绘制，而不是只在顶部显示它们的和。
* `setDrawHighlightArrow(boolean enabled)`: 设置为true时，高亮数据时将会在每个柱形上绘制高亮的箭头。

### PieChart

* `setDrawSliceText(boolean enabled)`:设置为true时，将会在扇区里绘制横轴数据。
* `setUsePercentValues(boolean enabled)`: 如果启用，图表里的数据将会以百分比的形式绘制，而不是用原始的值。提供的数据要经`ValueFormatter`格式化成百分比的形式。
* `setCenterText(SpannableString text)`:设置在`PieChart`中央显示的文本。为了避免绘制到扇区，过长的文本会被自动压缩。
* `setCenterTextRadiusPercent(float percent)`: 设置中央矩形文本框占饼状图中心的空洞的比例，默认1.f（100%）。
* `setHoleRadius(float percent)`: 设置饼状图中央空洞半径占饼状图半径的比例，默认50%。
* `setTransparentCircleRadius(float percent)`: 设置饼状图中与中央空洞相邻的透明圆环外半径栈饼状图半径的比例，默认值55%，意味着默认圆环比空洞大5%。
* `setTransparentCircleColor(int color)`:设置圆环的颜色。
* `setTransparentCircleAlpha(int alpha)`:设置圆环的透明度（0-255）。
* `setMaxAngle(float maxangle)`:设置饼状图的最大角度。360f意味着一个完整的饼状图，180f表示一个半圆。默认值：360f。 

### RadaChart

* `setSkipWebLineCount(int count)`: 允许跳过来自图表中心的网络线。在有大量线条时很有用。

## 12. 图例(Legend)

默认情况下，所有类型的图表都支持图例，能在给图表设置数据后自动绘制一个图例。图例通常包含多个条目，每个条目由一个标签和一个表格/形状表示。

图例包含的自动生成的条目的数量取决于`DataSet`的标签数量和不同颜色的数量（在所有`DataSet`对象范围内）。图表中图例的标签取决于给图表对象设置的标签。如果没有给`DataSet`指定标签，图表将会自动为它们生成标签。如果一个`DataSet`使用了多种颜色，这些颜色将被聚集起来，并只在一个标签上显示。

为了定制化图例，您可以通过使用`getLegend()`方法来获取`Legend`对象。

```java
Legend legend = chart.getLegend();
```

### 控制是否绘制图例

* `setEnabled(boolean enabled)`:设置启用或禁用图例。如果禁用，图例将不会被绘制。

### 修改图例样式

* `setTextColor(int color)`: 设置图例标签的颜色。
* `setTextSize(float size)`:以`dp`为单位，设置图例标签的文本大小。
* `setTypeface(Typeface tf)`: 给图例标签设置一个`Typeface`。

### 避免换行/越界

* `setWordWrapEnabled(boolean enabled)`:如果启用，图例内容不会超越图表边界，而是会在下一行绘制。请注意，这将降低性能，而且只在图例位于图表下方时可用。
* `setMaxSizePercent(float maxSize)`:设置相对于整个图表视图，图例最大尺寸的百分比。默认值：0.95f(95%)。

### 定制化图例

* `setPosition(LegendPosition pos)`: 设置图例显示的位置。可以在`RIGHT_OF_CHART`,`RIGHT_OF_CHART_CENTER`,`RIGHT_OF_CHART_INSIDE`,`BELOW_CHART_LEFT`,`BELOW_CHART_RIGHT`,`BELOW_CHART_CENTER`,或者`PIECHART_CENTER`（仅`PieChart`）等中选择。
* `setForm(LegendForm shape)`:设置要使用的`LegendForm`。这是紧邻图例标签用`DataSet`的颜色绘制的形状。可以在`SQUARE`,`CIRCLE`或者`LINE`中选择。
* `setFormSize(float size)`: 以`dp`为单位，设置图例形状的大小。
* `setXEntrySpace(float space)`:设置在水平方向上，图例条目的间距。
* `setYEntrySpace(float space)`:设置在竖直方向上，图例条目的间距。 
* `setFormToTextSpace(float space)`: 设置图例标签和图例形状的间距。
* `setWordWrapEnabled(boolean enabled)`:图例是否可以换行？当前仅支持`BelowChartLeft`,`BelowChartRight`,`BelowChartCenter`。 

### 设置自定义的标签和颜色

* `setCustom(int[] colors, String[] labels)`:设置自定义的图例标签和颜色数组。颜色数量要与标签数量匹配。每个颜色将绘制在和它索引相同的形状上。这将禁用根据数据集自动计算图例标签和颜色。调用`resetCustom()`可以重新启用（然后还要调用`notifyDataSetChanged()`来重新计算图例）。
* `resetCustom()`: 调用此方法将禁用自定义图例标签（由`setCustom(...)`设置）。取而代之的是，标签将被自动计算（在调用`notifyDataSetChanged()`后）。
* `setExtra(int[] colors, String[] labels)`:设置在自动计算的标签，颜色后追加的标签和颜色（如果图例已经完成计算，您需要调用`notifyDataSetChanged()`使改动生效）。

### 示例

```java
    Legend l = chart.getLegend();
    l.setFormSize(10f); // set the size of the legend forms/shapes
    l.setForm(LegendForm.CIRCLE); // set what type of form/shape should be used
    l.setPosition(LegendPosition.BELOW_CHART_LEFT);
    l.setTypeface(...);
    l.setTextSize(12f);
    l.setTextColor(Color.BLACK);
    l.setXEntrySpace(5f); // space between the legend entries on the x-axis
    l.setYEntrySpace(5f); // space between the legend entries on the y-axis
    // set custom labels and colors
    l.setCustom(ColorTemplate.VORDIPLOM_COLORS, new String[] { "Set1", "Set2", "Set3", "Set4", "Set5" });
    // and many more...
```

## 13. 描述(Description)

*描述指的是通常显示在图表右下角（默认值）的文本。如有需要，它可以提供额外的信息来展示图表数据。*

### 用法

可以用下面的方法来获取`Description`对象:

```java
Description description = chart.getDescription();
```

### 样式

下面的方法可以用来修改描述的样式：

```java
// enable or disable the description
description.setEnabled(...);
// set the description text
description.setText("...");
// set the position of the description on the screen
description.setPosition(float x, float y);
```

## 14. 动态实时数据

MPAndroid是免费软件，*官方不支持动态实时数据*。

## 15. 修改视区

本库由大量方法用于修改视区（视区是图表上可见的，正对图表的区域）。请注意这些方法这在`LineChart`,`BarChart`,`ScatterChart`和`CandleStickChart`上可用。

下面提到的方法由`Chart`类实现。另一种修改视区的方式是直接通过`ViewPortHandler`访问它（不使用由`Chart`类提供的间接访问方法）。只推荐熟悉API的高级用户使用这种方式。

> *请注意，只有在给`Chart`设置数据后，才能调用其**修改视区的方法**。*

### 限制可视内容

* `setVisibleXRangeMaximum(float maxXRange)`:设置一次性最大可见区域的大小（在横轴范围上）。举个例子，如果设为10，那么在不进行滚动的情况下，横轴上可见数据不会超过10个。
* `setVisibleXRangeMinimum(float minXRange)`: 设置一次性最小可见区域的大小（在横轴范围上）。举个例子，如果设为10，那么在横轴上不可能通过放大显示少于10个的数据。
* `setVisibleYRangeMaximum(float maxYRange, AxisDependency axis)`: 设置一次性最大可见区域的大小（在纵轴范围上）。您还需要提供该约束作用于哪个纵轴。
* `setViewPortOffsets(float left, float top, float right, float bottom)`:为当前视图设置自定义的偏移量（和实际图表窗口边缘的偏移）。设置该项将禁用自动计算偏移量。可通过`resetViewPortOffsets()`方法撤销。**只在您有确切需要时调用该方法**。
* `resetViewPortOffsets()`: 重置所有通过`setViewPortOffsets(...)`方法设置的自定义偏移量。允许图表再次自动计算偏移量。
* `setExtraOffsets(float left, float top, float right, float bottom)`:设置追加在自动计算偏移量后面的额外偏移量。这不会改变自动计算的偏移量，但会给它们增加额外的空间。

### 移动视图

* `fitScreen()`:重置所有缩放和拖拽，使图表恰好适合它的边界（最小化）。
* `moveViewToX(float xValue)`:把当前视区的左边缘移动到指定横坐标处。
* `moveViewToY(float yValue, AxisDependency axis)`: 在指定的纵轴上，居中给定的纵坐标值。
* `moveViewTo(float xValue, float yValue, AxisDependency axis)`:等效于调用`moveViewToX`()和`moveViewToY()`（确保和`setVisibleXRange(...)`还有`setVisibleYRange(...)`一起调用）。
* `centerViewTo(float xValue, float yValue, AxisDependency axis)`:这将移动当前视区的中心到指定横纵坐标处（确保和`setVisibleXRange(...)`还有`setVisibleYRange(...)`一起调用）。

### 伴随着动画移动视图

（于v2.2.3发布版添加）

* `moveViewToAnimated(float xValue, float yValue, AxisDependency axis, long duration)`:同`moveViewTo()`,会有指定时长的动画效果。
* `centerViewToAnimated(float xValue, float yValue, AxisDependency axis, long duration)`:同`centerViewTo`,会有指定时长的动画效果。

注意：所有的`moveViewTo(...)`方法都将自动调用`invalidate()`方法来刷新图表。因此没必要再调用`invalidate()`。

### 缩放（由程序控制）

* `zoomIn()`:自图表中心放大至原来的1.4倍。
* `zoomOut()`: 自图表中心缩小到原来的0.7倍。
* `zoom(float scaleX, float scaleY, float x, float y)`: 根据给定的缩放系数放大或缩小视区。x，y表示以像素为单位的缩放中心坐标。记住缩放系数为1f时等价于不缩放。
* `zoom(float scaleX, float scaleY, float xValue, float yValue, AxisDependency axis)`:根据给定的缩放系数放大或缩小视区。xValue，yValue表示缩放中心的实际数据值。记住缩放系数为1f时等价于不缩放。

### 伴有动画的缩放

（于v2.2.3发布版添加）

* `zoomAndCenterAnimated(float scaleX, float scaleY, float xValue, float yValue, AxisDependency axis, long duration)`:以伴有动画的方式，按指定系数，缩放中心缩放视区。

### 完整示例

```java
chart.setData(...); // first set data
// now modify viewport
chart.setVisibleXRangeMaximum(20); // allow 20 values to be displayed at once on the x-axis, not more
chart.moveViewToX(10); // set the left edge of the chart to x-index 10
// moveViewToX(...) also calls invalidate()
```

## 16. 动画

所有图表类型都支持动画，它可以用来以一种很棒的方式创建/建立图表。有三个方法用来设置横纵轴上动画的持续时间：

* `animateX(int durationMillis)`:在横轴上绘制图表数据，这意味着图表会在指定时间内从左到右建立。
* `animateY(int durationMillis)`: 在纵轴上绘制图表数据，这意味着图表会在指定时间内自下而上建立。
* `animateXY(int xDuration, int yDuration)`:在横纵坐标轴方向上绘制数据，这会使图表从左到右，自下而上建立。

```java
mChart.animateX(3000); // animate horizontal 3000 milliseconds
// or:
mChart.animateY(3000); // animate vertical 3000 milliseconds
// or:
mChart.animateXY(3000, 3000); // animate horizontal and vertical 3000 milliseconds
```

如果调用了任意动画方法，则无需调用`invalidate()`方法来刷新图表。

### 动画缓冲

本库允许您给您的动画应用合适的缓冲函数。您可以从以下静态预定义`Easing.EasingOption`中选择：

```java
  public enum EasingOption {
      Linear,
      EaseInQuad,
      EaseOutQuad,
      EaseInOutQuad,
      EaseInCubic,
      EaseOutCubic,
      EaseInOutCubic,
      EaseInQuart,
      EaseOutQuart,
      EaseInOutQuart,
      EaseInSine,
      EaseOutSine,
      EaseInOutSine,
      EaseInExpo,
      EaseOutExpo,
      EaseInOutExpo,
      EaseInCirc,
      EaseOutCirc,
      EaseInOutCirc,
      EaseInElastic,
      EaseOutElastic,
      EaseInOutElastic,
      EaseInBack,
      EaseOutBack,
      EaseInOutBack,
      EaseInBounce,
      EaseOutBounce,
      EaseInOutBounce,
  }
```

基本上，有两种方式来缓冲您的动画：

#### **1. 预定义缓冲选项**（可在所有Android版本上运行）

```java
 public void animateY(int durationmillis,  Easing.EasingOption option); 
```

给任意动画函数传入预定义缓冲选项：

```java
 // animate both axes with easing
 mChart.animateY(3000,  Easing.EasingOption.EaseOutBack); 
```

当您的程序在Android 3.0（API 11）以下运行时，请务必一直使用`Easing.EasingOption`作为预定义动画缓冲选项。

#### **2. 自定义缓冲函数**（自定义缓冲函数在Android 3.0以下版本会崩溃）

```java
public void animateY(int durationmillis, EasingFunction function);
```

通过创建实现`EasingFunction`方法的类，来**创建您自定义的**缓冲函数：

```java
 /**
  * Interface for creating custom made easing functions. 
  */
  public interface EasingFunction {
     /**
      * Called everytime the animation is updated.
      * @param input - the time passed since the animation started (value between 0 and 1)
      */
      public float getInterpolation(float input);
  }
```

然后用这种方式调用（注意，这在Android 3.0以下无法运行，将导致程序崩溃）：

```java
 // animate both axes with easing
 mChart.animateY(3000, new MyEasingFunction()); 
```

## 17. 标记视图（弹出视图）

*于v3.0.0发布版添加，`IMarker`表示图表中的标记（弹出视图）*

### IMarker接口

这个接口允许您创建在图条中高亮条目上像是的自定义标记视图。下面是这个接口提供的方法：

```java
public interface IMarker {
    /**
     * @return The desired (general) offset you wish the IMarker to have on the x- and y-axis.
     *         By returning x: -(width / 2) you will center the IMarker horizontally.
     *         By returning y: -(height / 2) you will center the IMarker vertically.
     */
    MPPointF getOffset();
    /**
     * @return The offset for drawing at the specific `point`. This allows conditional adjusting of the Marker position.
     *         If you have no adjustments to make, return getOffset().
     *
     * @param posX This is the X position at which the marker wants to be drawn.
     *             You can adjust the offset conditionally based on this argument.
     * @param posY This is the X position at which the marker wants to be drawn.
     *             You can adjust the offset conditionally based on this argument.
     */
    MPPointF getOffsetForDrawingAtPos(float posX, float posY);
    /**
     * This method enables a specified custom IMarker to update it's content every time the IMarker is redrawn.
     *
     * @param e         The Entry the IMarker belongs to. This can also be any subclass of Entry, like BarEntry or
     *                  CandleEntry, simply cast it at runtime.
     * @param highlight The highlight object contains information about the highlighted value such as it's dataset-index, the
     *                  selected range or stack-index (only stacked bar entries).
     */
    void refreshContent(Entry e, Highlight highlight);
    /**
     * Draws the IMarker on the given position on the screen with the given Canvas object.
     *
     * @param canvas
     * @param posX
     * @param posY
     */
    void draw(Canvas canvas, float posX, float posY);
}
```

创建标记视图

为了创建您的标记视图，您需要新建一个实现了`IMarker`接口的类：

```java
public class YourMarkerView implements IMarker { ... }
```

除了实现`IMarker`接口，您还可以通过继承下面某个预定义标记视图的方式创建您自己的类。这种方式更简易，不需要实现`IMarker`接口的所有方法。通常只有特定的方法需要重写和定制。最重要的是要主动覆盖`refreshContent(...)`方法，让标记视图绘制适配要绘制的数据。下面是一个简单的例子：

```java
public class YourMarkerView extends MarkerView {
    private TextView tvContent;
    public MyMarkerView(Context context, int layoutResource) {
        super(context, layoutResource);
        // find your layout components
        tvContent = (TextView) findViewById(R.id.tvContent);
    }
    // callbacks everytime the MarkerView is redrawn, can be used to update the
    // content (user-interface)
    @Override
    public void refreshContent(Entry e, Highlight highlight) {
        tvContent.setText("" + e.getY());
        // this will perform necessary layouting
        super.refreshContent(e, highlight);
    }
    private MPPointF mOffset; 
    @Override
    public MPPointF getOffset() {
        if(mOffset == null) {
           // center the marker horizontally and vertically
           mOffset = new MPPointF(-(getWidth() / 2), -getHeight());
        }
        return mOffset;
    }
}
```

### 获取/设置标记

用`setMarker()`来设置您图表的标记：

```java
IMarker marker = new YourMarkerView();
chart.setMarker(marker);
```

用`getMarker()`方法获取设置给图表的标记：

```java
IMarker marker = chart.getMarker();
```

### 预定义标记

简便起见，除了创建您自定义的标记视图外，本库还提供了一些预定义标记。这包括：

* [MarkerView](https://github.com/PhilJay/MPAndroidChart/blob/master/MPChartLib/src/main/java/com/github/mikephil/charting/components/MarkerView.java):基础标记。允许在图表表层标记区域渲染提供的布局资源。可通过继承此类并重写`refreshContent(...)`方法来适配标记数据。
* [MarkerImage](https://github.com/PhilJay/MPAndroidChart/blob/master/MPChartLib/src/main/java/com/github/mikephil/charting/components/MarkerImage.java):用于绘制图像的标记。允许在图表表层标记区域渲染提供的布局资源。可通过继承此类并重写`refreshContent(...)`方法来适配标记数据。

### 传统的`MarkerView`

在[v3.0.0](https://github.com/PhilJay/MPAndroidChart/releases)之前的版本，`MarkerView`类负责图表中高亮位置标记的绘制。更多关于此类的信息，请访问旧的[MarkerView 维基页面](https://github.com/PhilJay/MPAndroidChart/wiki/MarkerView)。

## 18. ChartData类

本维基条目旨在更好的了解*MPAndroid*背后**数据模型**。

`ChartData`是所有数据类（比如`LineData`,`BarData`...）的基类。它用于通过图表的`setData(...)`方法给图表提供数据。

```java
public class LineData extends ChartData { ...
```

下面提到的方法都已在`ChartData`类中实现，因此可用于所有子类。

### 设置数据样式

* `setValueTextColor(int color)`:给这个数据对象包含的所有数据集的文本设置的颜色（值标签绘制的颜色）。
* `setValueTextColors(List colors)`:设置一组颜色。
* `setValueTextSize(float size)`:以`dp`为单位，给这个数据对象所包含的所有数据集的文本设置大小。
* `setValueTypeface(Typeface tf)`:给这个数据对象所包含的所有数据集的值标签设置`Typeface`。
* `setValueFormatter(ValueFormatter f)`:给这个数据对象所包含的所有数据集设置一个自定义的`ValueFormatter`。
* `setDrawValues(boolean enabled)`:启用/禁用绘制这个数据对象所包含的所有数据集的值（以文本形式）。 

### 获取/常用方法

* `getDataSetByIndex(int index)`:返回在`DataSet`列表中给定的索引的`DataSet`对象。
* `contains(Entry entry)`:检查这个数据对象是否包含指定的条目。如果包含，则返回true，否则返回false。注意：此函数性能很差，不要再对性能有较高要求的场景下调用。
* `contains(T dataSet)`: 如果这个数据对象包含给定的`DataSet`，返回true，否则返回false。

### 清除

* `clearValues()`:删除所有`DataSet`对象的数据和条目。但不会移除提供的横坐标值。

### 高亮

* `setHighlightEnabled(boolean enabled)`: 设置为true的话将允许通过触摸高亮这个`ChartData`对象和其包含的所有数据集。
* `setDrawVerticalHighlightIndicator(boolean enabled)`:启用/禁用竖直高亮指示线。如果禁用，则不会绘制指示线。
* `setDrawHorizontalHighlightIndicator(boolean enabled)`:启用/禁用水平高亮指示线，如果禁用，则不会绘制指示线。

### 动态数据

* `notifyDataChanged()`:通知数据对象其基础数据已更改，并执行所有必要的重写计算操作。

有关从已有数据对象中添加/移除数据的方式，请查阅*动态实时数据*章节。

## 19. ChartData子类

本维基条目主要关注`ChartData`类的子类。所有没提到的`ChartData`类子类，都没有提供特别的扩展。

### BarData

* `setGroupSpace(float percent)`:设置不同柱状图组的间距占一个柱形宽度的百分比。spae = 100表示一个柱形的宽度。默认值：80。
* `isGrouped()`:如果数据对象已分组（包含一个以上的`DataSet`），则返回true，否则返回false。

### ScatterData

* `getGreatestShapeSize()`: 返回这个数据对象包含的所有数据集中最大的形状尺寸。

### PieData

* `getDataSet()`:返回设置给这个数据对象的`PieDataSet`对象。`PieData`对象不萌包含多个`PieDataSet`。
* `setDataSet(PieDataSet set)`: 给这个数据对象设置其将表示的`PieDataSet`。

### BubbleData

* `setHighlightCircleWidth(float width)`:以`dp`为单位，给这个数据对象包含的所有`BubbleDataSet`设置高亮装填下环绕气泡的圆环宽度。

### CombinedData

这个数据对象设计用来包含所有其他的数据对象。使用`setData()`方法给这个对象提供数据。只用于`CombinedChart`。

这是这个类的内部情况：

```java
public class CombinedData extends ChartData {
    // ...
    public CombinedData(List<String> xVals) { ... }
    public CombinedData(String[] xVals) { ... }
    public void setData(LineData data) { ... }
    public void setData(BarData data) { ... }
    public void setData(ScatterData data) { ... }
    public void setData(CandleData data) { ... }
    // ...
}
```

## 20. DataSet类

`DataSet`类是所有数据集类（`LineDataSet`,`BarDataSet`,...）的基类。

```java
public class LineDataSet extends DataSet { ...
```

`Data`类表示图表中同一归属的一组或一类条目（`Entry`）。它用来在逻辑上区分图表内不同组的数据（例如，`LineChart`内某条线，`BarChart`内某组柱形）。

下面提到的方法`DataSet`类均已实现，因此可用于所有子类：

### 设置数据样式

* `setValueTextColor(int color)`: 给这个`DataSet`对象设置文本值的颜色（绘制值标签的颜色）。
* `setValueTextColors(List colors)`:设置一组用于值颜色的颜色。 
* `setValueTextSize(float size)`:以`dp`为单位，给这个`DataSet`对象设置文本值大小。
* `setValueTypeface(Typeface tf)`:给这个`DataSet`对象设置所有值标签的`Typeface`。
* `setValueFormatter(ValueFormatter f)`:给这个`DataSet`对象设置自定义的`ValueFormatter`。
* `setDrawValues(boolean enabled)`:启用/禁用绘制这个`DataSet`的值。

如果您的数据对象中所有的值需要相同设置（比如颜色），您可以调用之前提到过的`ChartData`对象中提到的相关方法。

### 高亮

* `setHighlightEnabled(boolean enabled)`: 设置为true时将允许通过触摸高亮该`DataSet`。
* `setDrawVerticalHighlightIndicator(boolean enabled)`:启用/禁用竖直高亮指示线。如果禁用，则不会绘制指示线。
* `setDrawHorizontalHighlightIndicator(boolean enabled)`:启用/禁用水平高亮指示线。如果禁用，则不会绘制指示线。

### 获取/常用方法

* `contains(Entry entry)`:检查`DataSet`对象是否包含指定的`Entry`。如果包含，则返回true，否则返回false。注意：此函数性能很差，请不要在对性能有较高要求的场景下过度使用。

## 21. DataSet子类

本维基条目主要关注`ChartSet`类的子类。所有未涉及的`ChartSet`子类都没有特殊的扩展。

### Line-, Bar-, Scatter-, Bubble- & CandleDataSet（这些类均可使用下面提到的方法）

* `setHighLightColor(int color)`:设置绘制高亮指示器使用的颜色。别忘了使用`getResource().getColor(...)`或者`Color.rgb(...)`获取颜色（或者使用预定义的`Color.BLACK...`）。

### Line-, Bar-, Scatter-, Candle- & RadarDataSet

* `setDrawHighlightIndicators(boolean enabled)`:启用/禁用竖直和水平高亮指示器。如果需要单独配置，可调用`setDrawVerticalHighlightIndicator(...)`,`setDrawHozizontalHighlightIndicator(...)`。
* `setHighlightLineWidth(float width)`：以`dp`为单位，设置高亮线条（准线）的宽度。 

### Line- & RadarDataSet

* `setFillColor(int color)`:设置填充线曲面的颜色。
* `setFillAlpha(int alpha)`:设置填充线曲面的透明度值（0-255）。默认值：85，255 = 不透明，0  = 完全透明。
* `setFillDrawable(Drawable d)`:设置覆盖填充区域的`Drawable`。这也允许使用渐变。
* `setDrawFilled(boolean filled)`:如果数据集需要被完全填充而不是只绘制一条线的话，设置为true。禁用将提升性能。其注意，此方法调用`canvas.clipPath(...)`方法绘制填充区域。对于API < 18（Android 4.3）的设备，图表的硬件加速应被关闭。默认值：false。
* `setLineWidth(float width)`:设置数据集线条的宽度（最小 = 0.2f， 最大=10f）。默认值为1f。注意：线条越细，性能越好，线条越粗，性能越差。

### LineDataSet

* `setCircleRadius(float size)`:设置原型数据指示器的半径，默认值=4f。
* `setDrawCircles(boolean enabled)`:设置为true的话将启用绘制圆形指示器。默认值：true。
* `setDrawCubic(boolean enabled)`:如果设置为true，将会以立方样式绘制线条，而不是折线。**这将降低性能！**默认值：false。
* `setCubicIntensity(float intensity)`:设置线条的立方效果（如果启用）。最大值 = 1f  = 立方效果很强，最小值 = 0.05f = 很弱的立方效果，默认值：0.2f。
* `setCircleColor(int color)`:设置数据集所有圆形指示器的颜色。
* `setCircleColors(List colors)`:设置外圈圆的颜色。还有很多设置圆颜色的方法。
* `setCircleColorHole(int color)`:设置内圈圆的颜色。
* `setDrawCircleHole(boolean enabled)`:设置数据集的每个圆是否会绘制内圈圆。如果设为false，圆将以填充的方式绘制（没有洞）。
* `enableDashedLine(float lineLength, float spaceLength, float phase)`:启用以虚线模式绘制线条。比如这样“- — - —-”。`lineLength`是线段的长度，`spaceLength`是线段的间距，`phase`是以度单位的偏移值（通常用0）。

### BarDataSet

* `setBarSpacePercent(float percent)`:设置柱形图之间的间距（以柱形宽度的百分比表示）。
* `setBarShadowColor(int color)`:设置用于绘制柱形阴影的颜色。柱形印应是在柱形下层表示最大值的曲面。别忘了使用`getResource().getColor(...)`或者`Color.rgb(...)`获取颜色。
* `setHighLightAlpha(int alpha)`:设置用于绘制高光指示器的alpha值（透明度）。最小值=0（完全透明），最大值=255（完全不透明）。
* `setStackLabels(String[] labels)`:设置柱状图堆栈不同值的标签（如果有的话）。

### ScatterDataSet

* `setScatterShapeSize(float size)`:以像素为单位，设置散点图绘制的图形大小。这仅适用于非自定义形状。
* `setScatterShape(ScatterShape shape)`:设置将要在数据位置绘制的形状。

### CandleDataSet

* `setBodySpace(float space)`:设置每个蜡烛体左右两侧的间距。默认值：0.1f（10%），最大值0.45f，最小值0f。
* `setShadowWidth(float width)`:以`dp`为单位设置蜡烛阴影线的宽度。默认值：3f。
* `setShadowColor(int color)`:设置蜡烛阴影线的颜色。
* `setDecreasingColor(int color)`:设置当打开 > 关闭时数据集唯一使用的颜色。
* `setIncreasingColor(int color)`:设置当打卡 <= 关闭时数据集唯一使用的颜色。
* `setDecreasingPaintStyle(Paint.Style style)`:设置打开>关闭时的绘制样式（填充或笔划）。
* `setIncreasingPaintStyle(Paint.Style style)`:设置打开<=关闭时的绘制样式（填充或笔划）

关于`CandleDataSet`颜色的信息：通用的`setColors`(...),`setColor(...)`...等方法在此类上同样可用。如果要指定某部分（蜡烛体，阴影，...）的颜色，则需要使用上面提到的颜色。

### BubbleDataSet

* `setHighlightCircleWidth(float width)`:以像素为单位设置处于搞了状态时，环绕气泡的圆的宽度。

### PieDataSet

* `setSliceSpace(float degrees)`:以`dp`为单位设置饼状图扇区的间距。默认值：0 -> 没有间距，最大值 20，最小值 0。
* `setSelectionShift(float shift)`:设置饼状图中高亮显示的扇区离开图表中心的距离，默认值12f。

## 22. ViewPortHandler

`ViewPortHandler`类负责处理图表的视区。这意味着它负责图表视图中的可见部分，它当前与平移/缩放级别相关的状态，图标的大小，绘制区域和当前偏移量。`ViewPortHandler`可以直接访问上述属性并修改它们。

不像通过`Chart`类修改修改视区，直接修改`ViewPortHandler`不总是一种完全完全更改可见内容的方法，应由熟悉API的人员小心执行。错误的调用会导致不可预期的表现。无论如何,`ViewPortHandler`提供更多修改视区的高级方法。

### 获取实例

一个`ViewPortHandler`实例只能通过如下方式获取：

```java
ViewPortHandler handler = chart.getViewPortHandler();
```

### 缩放和平移

* `getScaleX()`:返回当前横轴的缩放级别。
* `getScaleY()`:返回当前纵轴的缩放级别。
* `getTransX()`:返回当前横轴的平移量。
* `getTransY()`:返回当前纵轴的平移量。

### 图表尺寸和内容

* `getChartWidth()`:返回图表宽度。
* `getChartHeight()`:返回图表高度。
* `getContentRect()`: 在一个`RectF`对象中，返回当前区域内容。

更多方法请查阅*JavaDocs*或者学习API。

## 23. 自定义填充线位置

`FillFormatter`接口**允许定制**`LineDataSet`中填充线结束的位置。要做的是创建一个新类，并实现`FillFormatter`接口。使用接口的

```java
public float getFillLinePosition(LineDataSet dataSet, LineDataProvider provider)
```

方法可以实现定制填充线结束位置计算的逻辑。

创建一个实现了接口的类：

```java
public class MyCustomFillFormatter implements FillFormatter {
    @Override
    public float getFillLinePosition(LineDataSet dataSet, LineDataProvider dataProvider) {
        float myDesiredFillPosition = ...;
        // put your logic here...
        return myDesiredFillPosition;
    }
}
```

然后将自定义的格式化器设置给您的`LineDataSet`:

```java
lineDataSet.setFillFormatter(new MyCustomFillFormatter());
```

## 24. Xamarin

**Xamarin可使用MPAndroidChart**

Xamarin端口（由 [Flash3001](https://github.com/Flash3001)维护）：Android— [GitHub](https://github.com/Flash3001/MPAndroidChart.Xamarin)/[NuGet](https://www.nuget.org/packages/MPAndroidChart/)。iOS—[GitHub](https://github.com/Flash3001/iOSCharts.Xamarin)/[NuGet](https://www.nuget.org/packages/iOSCharts/)。

## 25. 创建自定义数据集

从[v2.2.0](https://github.com/PhilJay/MPAndroidChart/releases)开始，MPAndrodChart允许您轻松创建并在图表中使用自定义的`DataSet`对象。

### 您需要做什么

* 创建您自定义的类（例如`CustomDataSet`）。
* 让它继承`BaseDataSet<? extends Entry>`。
* 根据您的需求，让它实现某个`IDataSet`接口（例如`IBarDataSet`）。

* 实现所有需要的方法，并让它们返回您需要的值。

### 示例

创建一个用于`BarChart`的自定义`BarDataSet`：

```java
public class CustomBarDataSet extends BaseDataSet<BarEntry> implements IBarDataSet {
    // implement all by the extended class and interface required methods
}
```

在创建`CustomBarDataSet`并实现所有接口需要的方法后，它可以像普通`BarDataSet`那样用于任意`BarChart`。

## 26. 杂项

### 图表内容

* `clear()`:清除图表的所有数据（通过将其数据对象设为null）。调用`invalidate()`刷新图表。
* `clearValues()`:清空所有数据对象（通过空所有条目）。不会移除已有的横坐标值。调用`invalidate()`刷新图表。
* `isEmpty()`:如果图表的数据对象为null，或者没有数据条目时返回空。

### 有用的获取方法

* `getData()`:返回设置给图表的数据对象。
* `getViewPortHandler()`:返回包含图表大小，边界（偏移量，内容区域），缩放，平移状态信息的`ViewPortHandler`对象。
* `getRenderer()`:返回负责渲染图表数据的主`DataRenderer`。
* `getCenter()`:返回整个图表的中心点。
* `getCenterOffsets()`:返回图表绘制区域的中心点。
* `getPercentOfTotal(float value)`: 返回给定的值占图表所有值的和的百分比。
* `getYMin()`:返回图表中最小的数据。
* `getYMax()`:返回图表中最大的数据。
* `getLowestVisibleXIndex()`:返回图表中可见的最小横轴索引（横轴上的值）。
* `getHighestVisibleXIndex()`:返回图表中可见的最大横轴索引（横轴上的值）。

### 更多方法（Chart类）

* `saveToGallery(String title)`:将当前图表状态以图片的形式保存到相册。**别忘了在manifest中添加"WRITE_EXTERNAL_STORAGE"权限**。
* `saveToPath(String title, String pathOnSD)`:将当前图表状态以图片的形式保存到指定路径。**别忘了在manifest中添加"WRITE_EXTERNAL_STORAGE"权限**。
* `getChartBitmap()`:返回表示图表的`Bitmap`对象，这个`Bitmap`总是包含图表最新的绘制状态。
* `setHardwareAccelerationEnabled(boolean enabled)`:启用/禁用硬件加速，只在API 11以上可用。

