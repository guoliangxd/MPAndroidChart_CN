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

### 通过程序高亮

### 选择回调

### Highlight类

### 定制高亮器

## 4. 坐标系

## 5. 横坐标

## 6. 纵坐标

## 7. 设置数据

## 8. 设置颜色

## 9. 格式化数据

## 10. 通用设置与样式

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









