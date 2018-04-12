### 如何理解Activity，View，Window三者之间的关系？

`Activity`像一个工匠（控制单元），`Window`像窗户（承载模型），`View`像窗花（显示视图）`LayoutInflater`像剪刀，`Xml`配置像窗花图纸。

* `Activity`构造的时候会初始化一个`Window`，准确的说是`PhoneWindow`。
* `PhoneWindow`有一个`ViewRoot`，这个`ViewRoot`是一个View或者说`ViewGroup`，是最初始的根视图。
* `ViewRoot`通过`addView`方法来一个个的添加`View`。比如`TextView`，`Button`等
* 这些View的事件监听，是由`WindowManagerService`来接受消息，并且回调`Activity`函数。比如`onClickListener`，`onKeyDown`等。

