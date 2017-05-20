#### 使用overridePendingTransition导致activity切换动画执行中黑屏的问题。

##### 一、场景描述

实现场景是：Activity A跳转到Activity B，B要从底部弹出。B关闭时，要从底部退出。

##### 二、实现方法

A跳转B时（startActivity之后添加）：

```
startActivity(intent)
overridePendingTransition(R.anim.anim_bottom_in, R.anim.anim_no)
```

B返回A时（在finish的super.finish()后添加）：

```
 @Override
    public void finish() {
        super.finish();
        overridePendingTransition(R.anim.anim_no, R.anim.anim_bottom_out);
    }
```

anim_bottom_in.xml

```
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android" >

    <translate
        android:duration="300"
        android:fromYDelta="100%"
        android:interpolator="@android:anim/decelerate_interpolator"
        android:toYDelta="0" />
</set>
```

anim_bottom_out.xml

```
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android" >

    <translate
        android:duration="250"
        android:fromYDelta="0"
        android:interpolator="@android:anim/decelerate_interpolator"
        android:toYDelta="100%" />
</set>
```

开始的时候为了不让A执行动画效果，因此设置如下：

```
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android" >

    <translate
        android:duration="0"
        android:fromYDelta="0"
        android:interpolator="@android:anim/decelerate_interpolator"
        android:toYDelta="0" />
</set>
```

但是A跳转到B时屏幕会黑一下。

解决办法： 
anim_no.xml

```
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android" >

    <translate
        android:duration="300"
        android:fromYDelta="0"
        android:interpolator="@android:anim/decelerate_interpolator"
        android:toYDelta="0" />
</set>
```

**重点在于duration,要保证与anim_bottom_in的时间一致，才不会有屏幕变黑。** 
**因为：** 
duration为0的意思是没有动画，没有动画的话，当前activity就要立刻变成不可见状态，所以如果你要当前动画不动不变黑，应该传一个静止的动画，比如透明度100到100，时间和其他动画参数的时间一样就可以了。