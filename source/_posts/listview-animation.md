title: listview item进入和删除动画
categories:
  - Android
date: 2014-11-24 22:08:59
tags:
  - listview
  - animation
 
---

### 1.先看一下最终效果

![listview](https://raw.githubusercontent.com/Reacoder/AndroidDevelopNote/master/listview_animation.gif)

### 2.从右边划入的动画
* 定义xml动画
```xml
<?xml version="1.0" encoding="utf-8"?>
<translate xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="@integer/animTime"
    android:fromXDelta="100%p"
    android:toXDelta="0%p" >

</translate>
```
* `Adapter`里只需要写如下代码
```java
    @Override
    public View getChildView(final int groupPosition, int childPosition, 
         boolean isLastChild, View convertView, ViewGroup parent) {
        ....

        if (!junk.isAnimatedBefore()) {
            junk.setAnimatedBefore(true);
            convertView.startAnimation(AnimationUtils.loadAnimation(mContext, R.anim.slide_left_in));
        }else{
            convertView.clearAnimation();
        }
        return convertView;
    }
```
### 3.从左边划出的动画
* 定义xml动画
```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:fillEnabled="true"
    android:fillAfter="true"
    android:duration="@integer/animTime">
    <translate
        android:fromXDelta="0%p"
        android:toXDelta="-100%p">

    </translate>
</set>
```
* `Adapter`里不需要做什么，需要在外面操作`ListView`.

### 4.在外面操作`ListView`
* 开始动画
```java
    private void startBoost() {
        mIsDeleting = true;
        boolean hasAnimation = false;
        long offset = 0;
        mAnimCount = 0;
        int groupPosition = -1;
        int childPosition = -1;
        Iterator<ArrayList<Junk>> listIterator = mChildren.iterator();
        while (listIterator.hasNext()) {
            groupPosition++;
            final Iterator<Junk> junkIterator = listIterator.next().iterator();
            while (junkIterator.hasNext()) {
                childPosition++;
                Junk junk = junkIterator.next();
                if (junk.isChecked()) {
                    ...
                    junkIterator.remove();
                    final View itemView = ListViewUtil.getChildItemView(this,
                                                     mListView, groupPosition, childPosition);
                    if (itemView != null) {
                        hasAnimation = true;
                        ((Animation) itemView.getTag(R.id.anim)).setAnimationListener(mAnimationListener);
                        ((Animation) itemView.getTag(R.id.anim)).setStartOffset(offset);
                        offset += 100;
                        itemView.startAnimation(((Animation) itemView.getTag(R.id.anim)));
                    }
                }
            }
        }
        if (!hasAnimation) {
            deleteCompleted();
        }
    }
```

* `mAnimationListener`定义如下：
```java
    private Animation.AnimationListener mAnimationListener = new Animation.AnimationListener() {
        @Override
        public void onAnimationStart(Animation animation) {
            mAnimCount++;
        }

        @Override
        public void onAnimationEnd(Animation animation) {
            mAnimCount--;
            if (mAnimCount == 0) {
                deleteCompleted();
            }
        }

        @Override
        public void onAnimationRepeat(Animation animation) {

        }
    };
```

* 删除完成后需要调用`ListViewUtil.clearListViewAnim(this, mListView);`,否则原来动画的地方会显示空白。
* 动画时要禁用`OnTouch`事件，因为我们做动画时并没有调用`mAdapter.notifyDataSetChanged()`，所以如果滚动，由于数据没有同步，肯定会挂掉。

**禁用`OnTouch`事件的方法如下**
```java
    @Override
    public boolean dispatchTouchEvent(MotionEvent ev) {
        // intercept touch event when deleting ,avoid listview get view.
        if (mIsDeleting) {
            return true;
        }

        return super.dispatchTouchEvent(ev);
    }
```

### 4.`ListView`的工具方法
```java
    public static View getChildItemView(Context context, FloatingGroupExpandableListView listView, int groupPosition, int childPosition) {
        long packedPosition = ExpandableListView.getPackedPositionForChild(groupPosition, childPosition);
        int flatPosition;
        try {
            flatPosition = listView.getFlatListPosition(packedPosition);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
        int firstPosition = listView.getFirstVisiblePosition();
        int wantedChild = flatPosition - firstPosition;
        if (wantedChild < 0 || wantedChild >= listView.getChildCount()) {
            return null;
        }
        View childItemView = listView.getChildAt(wantedChild);
        childItemView.setTag(R.id.anim, AnimationUtils.loadAnimation(context, R.anim.slide_left_out));
        return childItemView;
    }

    public static View getItemView(Context context, ListView listView, int position) {
        int firstPosition = listView.getFirstVisiblePosition() - listView.getHeaderViewsCount();
        int wantedChild = position - firstPosition;
        if (wantedChild < 0 || wantedChild >= listView.getChildCount()) {
            return null;
        }
        View wantedView = listView.getChildAt(wantedChild);
        wantedView.setTag(R.id.anim, AnimationUtils.loadAnimation(context, R.anim.slide_left_out));
        return wantedView;
    }

    public static void clearListViewAnim(Context context, ListView listView) {
        int first = listView.getFirstVisiblePosition();
        int count = listView.getChildCount() + 2;
        for (int i = first; i < first + count; i++) {
            View itemView = getItemView(context, listView, i);
            if (itemView != null) {
                itemView.clearAnimation();
            }
        }
    }
```






