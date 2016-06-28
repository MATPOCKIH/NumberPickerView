# NumberPickerView
another NumberPicker with more flexible attributes

##前言
在平时开发中会用到NumberPicker组件，但是默认风格的`NumberPicker`具有一些不灵活的属性，且定制起来比较麻烦，且缺少一些过渡动效，因此在应用开发时，一般采用自定义的控件来完成选择功能。
###控件截图

![Example Image][1]<br>
效果图

![Example Image][2]<br>
静态截图以及渐变效果

###说明
`NumberPickerView`是一款与android原生`NumberPicker`具有类似界面以及类似功能的`View`。
主要功能同样是从多个候选项中通过上下滚动的方式选择需要的选项，但是与`NumberPicker`相比较，有几个主要不同点，下面是两者的不同之处。

####原始控件特性-NumberPicker
1. 显示窗口只能显示3个备选选项；
2. 在fling时阻力较大，无法快速滑动；
3. 在选中与非选中状态切换比较生硬；
4. 批量改变选项中的内容时，没有动画效果；
5. 动态设置wrap模式时(`setWrapSelectorWheel()`方法)，会有“暂时显示不出部分选项”的问题；
6. 选中位置没有文字说明；
7. 代码中不能控制选项滑动滚动到某一item；

####自定义控件特性-NumberPickerView
1. 显示窗口可以显示多个备选选项；
2. fling时滑动速度较快，且可以设置摩擦力；
3. 在选中与非选中的状态滑动时，具有渐变的动画效果，包括文字放大缩小以及颜色的渐变；
4. 在批量改变选项中的内容时，可以选择是否采用友好的滑动效果；
5. 可以动态的设置是否wrap，即，是否循环滚动；
6. 选中位置可以添加文字说明，可控制文字字体大小颜色等；
7. 具有在代码中动态的滑动到某一位置的功能；
8. 支持`wrap_content`，支持item的padding
9. 提供多种属性，优化UI效果
10. 在滑动过程中不响应`onValueChanged()`
11. 点击上下单元格，可以自动滑动到对应的点击对象。
12. 兼容NumberPicker的重要方法和接口：
```
    兼容的方法有：
    setOnValueChangedListener()
    setOnScrollListener()
    setDisplayedValues()/getDisplayedValues()
    setWrapSelectorWheel()/getWrapSelectorWheel()
    setMinValue()/getMinValue()
    setMaxValue()/getMaxValue()
    setValue()/getValue()
    
    兼容的内部接口有：
    OnValueChangeListener
    OnScrollListener
```

###使用方法
1.导入至工程
```
    compile 'cn.carbswang.android:NumberPickerView:1.0.0'
```
或者
```
    <dependency>
      <groupId>cn.carbswang.android</groupId>
      <artifactId>NumberPickerView</artifactId>
      <version>1.0.0</version>
      <type>pom</type>
    </dependency>
```
2.通过布局声明NumberPickerView
```
    <cn.carbswang.android.library.NumberPickerView
        android:id="@+id/picker"
        android:layout_width="wrap_content"
        android:layout_height="240dp"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="20dp"
        android:background="#11333333"
        android:contentDescription="test_number_picker_view"
        app:npv_ItemPaddingHorizental="5dp"
        app:npv_ItemPaddingVertical="5dp"
        app:npv_ShowCount="5"
        app:npv_TextSizeNormal="16sp"
        app:npv_TextSizeSelected="20sp"
        app:npv_WrapSelectorWheel="true"/>

```
3.Java代码中使用：
  1)若设置的数据(String[] mDisplayedValues)不会再次改变，可以使用如下方式进行设置：（与NumberPicker的设置方式一致）
```
        picker.setMinValue(minValue);
        picker.setMaxValue(maxValue);
        picker.setValue(value);
```
  2)若设置的数据(String[] mDisplayedValues)会改变，可以使用如下组合方式进行设置：（与NumberPicker的更改数据方式一致）
```
        int minValue = getMinValue();
        int oldMaxValue = getMaxValue();
        int oldSpan = oldMaxValue - minValue + 1;
        int newMaxValue = display.length - 1;
        int newSpan = newMaxValue - minValue + 1;
        if (newSpan > oldSpan) {
            setDisplayedValues(display);
            setMaxValue(newMaxValue);
        } else {
            setMaxValue(newMaxValue);
            setDisplayedValues(display);
        }
```
    或者直接使用NumberPickerView提供的方法：
```    
    refreshByNewDisplayedValues(String[] display)
```
使用此方法时需要注意保证数据改变前后的minValue值不变。

4.另，NumberPickerView提供了平滑滚动的方法：<br>
    `public void smoothScrollToValue(int fromValue, int toValue, boolean needRespond)`<br>
    
此方法与`setValue(int)`方法相同之处是可以动态设置当前显示的item，不同之处在于此方法可以使`NumberPickerView`平滑的从滚动，即从`fromValue`值挑选最近路径滚动到`toValue`，第三个参数`needRespond`用来标识在滑动过程中是否响应`onValueChanged`回调函数。因为多个`NumberPickerView`在联动时，很可能不同的`NumberPickerView`的停止时间不同，如果在此时响应了`onValueChanged`回调，就可能再次联动，造成数据不准确，将`needRespond`置为`false`，可避免在滑动中响应回调函数。
    
###主要原理

####1.滚动效果的产生：
`Scroller` + `VelocityTracker` + `onDraw(Canvas canvas)`

####2.自动校准位置。
`Handler` 刷新当前位置

####3.渐变的UI效果
渐变UI效果同样是通过计算当前滑动的坐标以及某个item与中间显示位置的差值比例，来确定此item中的字体大小以及颜色。

###将NumberPicker改为NumberPickerView

要替代项目中使用的NumberPicker，只需要将涉及NumberPicker的代码（如回调中传入了NumberPicker、使用了NumberPicker的内部接口）改为NumberPickerView即可。

欢迎大家不吝指教。
email: yeah0126@yeah.net

## License

    Copyright 2016 Carbs.Wang (IndicatorView)

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.

[1]: https://github.com/Carbs0126/Screenshot/blob/master/numberpickerview.gif
[2]: https://github.com/Carbs0126/Screenshot/blob/master/numberpickerviewall.jpg