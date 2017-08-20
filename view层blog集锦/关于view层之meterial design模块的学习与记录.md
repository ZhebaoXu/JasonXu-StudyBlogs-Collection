#### 一、常用控件介绍：

AppBarLayout是一种支持响应滚动手势的app bar布局（比如工具栏滚出或滚入屏幕）；

CollapsingToolbarLayout则是专门用来实现子布局内不同元素响应滚动细节的布局。

**说明：**

1、与AppBarLayout组合的滚动布局（Recyclerview、NestedScrollView等）需要设置app:layout_behavior=”@string/appbar_scrolling_view_behavior”（上面代码中NestedScrollView控件所设置的）。**没有设置的话，AppBarLayout将不会响应滚动布局的滚动事件。**

2、CollapsingToolbarLayout和ScrollView一起使用会有滑动bug，注意要使用NestedScrollView来替代ScrollView。

3、AppBarLayout的子布局有5种滚动标识(就是上面代码CollapsingToolbarLayout中配置的app:layout_scrollFlags属性)：

- scroll:将此布局和滚动时间关联。这个标识要设置在其他标识之前，没有这个标识则布局不会滚动且其他标识设置无效。
- enterAlways:任何向下滚动操作都会使此布局可见。这个标识通常被称为“快速返回”模式。
- enterAlwaysCollapsed：假设你定义了一个最小高度（minHeight）同时enterAlways也定义了，那么view将在到达这个最小高度的时候开始显示，并且从这个时候开始慢慢展开，当滚动到顶部的时候展开完。
- exitUntilCollapsed：当你定义了一个minHeight，此布局将在滚动到达这个最小高度的时候折叠。
- snap:当一个滚动事件结束，如果视图是部分可见的，那么它将被滚动到收缩或展开。例如，如果视图只有底部25%显示，它将折叠。相反，如果它的底部75%可见，那么它将完全展开。

4、CollapsingToolbarLayout可以通过app:contentScrim设置折叠时工具栏布局的颜色，通过app:statusBarScrim设置折叠时状态栏的颜色。默认contentScrim是colorPrimary的色值，statusBarScrim是colorPrimaryDark的色值。这个后面会用到。

5、CollapsingToolbarLayout的子布局有3种折叠模式（Toolbar中设置的app:layout_collapseMode）

- off：这个是默认属性，布局将正常显示，没有折叠的行为。
- pin：CollapsingToolbarLayout折叠后，此布局将固定在顶部。
- parallax：CollapsingToolbarLayout折叠时，此布局也会有视差折叠效果。

当CollapsingToolbarLayout的子布局设置了parallax模式时，我们还可以通过app:layout_collapseParallaxMultiplier设置视差滚动因子，值为：0~1。

6、FloatingActionButton这个控件通过app:layout_anchor这个设置锚定在了AppBarLayout下方。FloatingActionButton源码中有一个Behavior方法，当AppBarLayout收缩时，FloatingActionButton就会跟着做出相应变化。