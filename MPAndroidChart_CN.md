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

为什么需要这样的理由是由于本库使用了二分查找算法，只有在有序的列表上才会有更好的表现。

### BarChart（柱状图）

### Grouped BarChart（分组柱状图）

### Stacked BarChart（堆叠柱状图）

### PieChart（饼状图）

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

## 12. 图例

## 13. 描述

## 14. 动态和实时数据

## 15. 修改视点

## 16. 动画

## 17. 标记视图

## 18. ChartData类

## 19. ChartData子类

## 20. DataSet类

## 21. DataSet子类

## 22. VIewPortHandler

## 23. 自定义填充线位置

## 24. Xamarin

## 25. 创建用户数据集

## 26. 杂项









