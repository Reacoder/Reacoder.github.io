title: textview带有右上角文字
categories:
  - Android
date: 2014-11-24 22:14:40
tags:
  - customView
  - textView
  - spannableString

---

![rightuptext pic](http://d.pcs.baidu.com/thumbnail/c75a444993a89f43fc58035402640681?fid=3440609375-250528-234267853765401&time=1416837600&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-lRKYokykN%2F9aUhi9WLnZPY5BeV4%3D&rt=sh&expires=2h&r=288789566&sharesign=unknown&size=c710_u500&quality=100)

### 1.`RightUpperCornerText` 代码
```java
public class RightUpperCornerText extends TextView {
    private Paint mPaint;
    private String mCornerText;
    private Context mContext;

    public RightUpperCornerText(Context context) {
        super(context);
        init(context);
    }

    public RightUpperCornerText(Context context, AttributeSet attrs) {
        super(context, attrs);
        init(context);
    }

    public RightUpperCornerText(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
        init(context);
    }

    private void init(Context context) {
        mContext = context;
        mPaint = new Paint();
        mPaint.setAntiAlias(true);
        mPaint.setColor(Color.WHITE);

        float cornerSize = getTextSize() * 0.6f;
        if (cornerSize > 50) {
            cornerSize = 50;
        }
        mPaint.setTextSize(cornerSize);
        mCornerText = "";
    }

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);

        // 不要忘了设置 Padding
        String text = (String) getText();
        Rect bounds = new Rect();
        getPaint().getTextBounds(text, 0, text.length(), bounds);
        canvas.drawText(mCornerText, getPaddingLeft() + bounds.right + bounds.left,
                (canvas.getHeight() + bounds.top) / 2 + mPaint.measureText(mCornerText),
                mPaint);

    }

    public void setCornertext(String text) {
        mCornerText = text;
    }

    public void setCornertextSize(float size) {
        mPaint.setTextSize(TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP,
                size, mContext.getResources().getDisplayMetrics()));
        invalidate();
    }
}
```

### 2. 用法
```xml
    <com.ilegendsoft.toprankapps.cleanmaster.UI.RightUpperCornerText
        android:id="@+id/dashboard_percentage"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="20dp"
        android:layout_centerInParent="true"
        android:textColor="@android:color/white"
        android:textSize="26sp" />
```

`mPercentageView.setCornertext(cornerText);`

**注意** `android:padding="20dp"`设置padding 很重要，否则没地方画了。

### 3. 其实完全没必要自定义，采用`TextView Style`可以实现各种样式的字体排版，就是`SpannableString`

先看下图片

![复合文本图片](http://orgcent.com/wp-content/uploads/2012/04/textview_spannablestring.jpg)


具体参考如下链接：

* [TextView使用SpannableString设置复合文本](http://orgcent.com/android-textview-spannablestring-span/)
* [Android文本样式](http://blog.csdn.net/lixin84915/article/details/8110667)




