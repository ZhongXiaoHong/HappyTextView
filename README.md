# HappyTextView

## 使用Html.fromHtml改变部分字体颜色

示例：

```js
         <TextView
                android:id="@+id/headerContentTv"
                android:layout_marginTop="@dimen/x7"
                android:textColor="@color/c666666"
                android:textSize="@dimen/x24"
                tools:text="扫码 32 次共优惠 132.48 元，赠积分 123 ，送券 5 张 "
                android:layout_width="wrap_content"
                android:layout_height="wrap_content" />
```
```java



    String html = String.format("扫码 %s 次共优惠 %s 元，赠积分 %s ，送券 %s 张",
                String.format("<font color=#333333>%s</font>", info.getTime()),
                String.format("<font color=#1a1a1a>%s</font>", info.getDiscountAmount()),
                String.format("<font color=#f0900a>%s</font>", info.getSourceNumber()),
                String.format("<font color=#089dfc>%s</font>", info.getGiftNumber())
                
    tv.setText(Html.fromHtml(html));

```
效果：
![这里写图片描述](https://github.com/ZhongXiaoHong/HappyTextView/blob/master/QQ%E6%88%AA%E5%9B%BE20191113164018.jpg?raw=true)


## 动态设置TextView各个方向的Drawable
示例：
```java
 TextView  btn = (TextView)v;
 Drawable drawable = ContextCompat.getDrawable(this,R.mipmap.ic_launcher);
 drawable.setBounds(0,0,70,70);//需要设置大小，否则不生效
 btn.setCompoundDrawables(drawable,null,null,null);

```
效果
![这里写图片描述](https://github.com/ZhongXiaoHong/HappyTextView/blob/master/18A0E398772946FDAC8E649A30C9F532.gif?raw=true)
