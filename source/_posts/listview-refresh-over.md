title: 如何确保Listview刷新结束,然后再执行其他操作。
categories:
  - Android
date: 2014-11-24 14:50:21
tags:
  - listview

---
**场景：**当调用`Listview` 的`notifyDataSetChanged()`刷新列表时，执行滚动或点击按钮删除源数据，然后再次刷新列表。

**方法：**采用`View`的`post()`方法,把对数据源的操作加入消息队列，等待刷新结束，如下：

```java
adapter.notifyDataSetChanged();
view.post( new Runnable() {
    @Override
    public void run() {
    //call smooth scroll
    }
  });
```