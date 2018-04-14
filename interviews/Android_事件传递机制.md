### 谈谈对事件分发机制的理解

所谓事件分发机制其实就是指当一个`MotionEvent`事件产生后, 系统需要把这个事件传递到具体的`View`而消费的过程. 点击事件的分发由三个方法共同完成:

* `pubic boolean dispatchTouchEvent(MotionEvent ev)` 用来进行事件的分发
* `public boolean onInterceptTouchEvent(MotionEvent ev)` 用来判断是否拦截某个事件
* `public boolean onTouchEvent(MotionEvent ev)` 用来处理点击事件

三者的关系大概为:

```java
public boolean dispatchTouchEvent(MotionEvent ev){
  boolean consume= false;
  if(onInterceptTouchEvent(ev)){
    consume= onTouchEvent(ev);
  }else{
    consume= child.dispatchTouchEvent(ev);
  }
}
```

下面从两个方面来阐述事件分发机制:

* 事件的传递过程
* 事件的传递规则

##### 1. 事件的传递过程

当一个点击事件产生后, 他的传递过程遵循如下顺序: `Activity`→`Window`→`View`. 即事件总是先传递给`Activity`,`Activity`再传递给`Window`, 具体来说是`PhoneWindow`, `PhoneWindow`再将事件传递个顶级`View`也就是`DecorView`. `DecorView`接收到事件后, 就会按照事件分发机制的规则去分发事件. 

##### 2. 事件传递的规则

对于一个根`ViewGroup`来说, 点击事件产生后, 首先会传递给他, 这时`ViewGroup`的`dispatchTouchEvent()`就会被调用, 如果这个`ViewGroup`的`onInterceptTouchEvent()`方法返回`true`就表示他要拦截事件, 接着这个事件就会交给这个`ViewGroup`处理, 即他的`onTouchEvent()`方法会被调用; 如果这个`ViewGroup`的`onInterceptTouchEvent()`方法返回`false`就表示他不拦截当前事件, 这时当前事件就会继续传递到他的子元素, 接着子元素的`dispatchTouchEvent()`方法就会被调用, 如此反复直到事件被最总消费. 

当一个`View`需要处理事件时, 如果他设置了`onTouchListener`,那么`OnTouchListener`中的`onTouch`方法就被调用. 这时事件如何处理还要看`onTouch`的返回值, 如果返回值为`false`则当前`View`的`onTouchEvent`就会被调用, 如果返回为`true`,那么`onTouchEvent()`方法将不会被调用.  如果给`View`设置了`OnClickListner`,那么在`onTouchEvent()`中`onClick()`方法会被调用. 三者调用的优先级为: `onTouchListener > onTouchEvent > onClickListener`.



