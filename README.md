# HappyTextView

## 使用Html.fromHtml改变部分字体大小
1.TextView中是不能识别size,所以需要自定义处理标签
```java
public class HtmlTagHandler  implements Html.TagHandler { 
义标签名称
    private String tagName;

    // 标签开始索引
    private int startIndex = 0;
    // 标签结束索引
    private int endIndex = 0;
    // 存放标签所有属性键值对
    final HashMap<String, String> attributes = new HashMap<>();

    public HtmlTagHandler(String tagName) {
        this.tagName = tagName;
    }

    @Override
    public void handleTag(boolean opening, String tag, Editable output, XMLReader xmlReader) {
        // 判断是否是当前需要的tag
        if (tag.equalsIgnoreCase(tagName)) {
            // 解析所有属性值
            parseAttributes(xmlReader);

            if (opening) {
                startHandleTag(tag, output, xmlReader);
            }
            else {
                endEndHandleTag(tag, output, xmlReader);
            }
        }
    }

    public void startHandleTag(String tag, Editable output, XMLReader xmlReader) {
        startIndex = output.length();
    }

    public void endEndHandleTag(String tag, Editable output, XMLReader xmlReader) {
        endIndex = output.length();

        // 获取对应的属性值
        String color = attributes.get("color");
        String size = attributes.get("size");
        size = size.split("px")[0];

        // 设置颜色
        if (!TextUtils.isEmpty(color)) {
            output.setSpan(new ForegroundColorSpan(Color.parseColor(color)), startIndex, endIndex,
                    Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        }
        // 设置字体大小
        if (!TextUtils.isEmpty(size)) {
            output.setSpan(new AbsoluteSizeSpan(Integer.parseInt(size)), startIndex, endIndex,
                    Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        }
    }

    /** * 解析所有属性值 * * @param xmlReader */
    private void parseAttributes(final XMLReader xmlReader) {
        try {
            Field elementField = xmlReader.getClass().getDeclaredField("theNewElement");
            elementField.setAccessible(true);
            Object element = elementField.get(xmlReader);
            Field attsField = element.getClass().getDeclaredField("theAtts");
            attsField.setAccessible(true);
            Object atts = attsField.get(element);
            Field dataField = atts.getClass().getDeclaredField("data");
            dataField.setAccessible(true);
            String[] data = (String[]) dataField.get(atts);
            Field lengthField = atts.getClass().getDeclaredField("length");
            lengthField.setAccessible(true);
            int len = (Integer) lengthField.get(atts);

            for (int i = 0; i < len; i++) {
                attributes.put(data[i * 5 + 1], data[i * 5 + 4]);
            }
        } catch (Exception e) {

        }
    }
}
```
2.设置进TextView
```java
 String minPrice = "10.99";
        String maxPrice = "100.33";
        String html = String.format("<p><strong>¥ <myfont size='60px'>%s</myfont> <strong> <myfont size='12px'>%s</myfont></P>",minPrice,
                maxPrice.equals(minPrice)? "" : " 起");
       tv.setText(Html.fromHtml(html,null, new SizeLabel("myfont")));          
```
效果：

![这里写图片描述](https://github.com/ZhongXiaoHong/HappyTextView/blob/master/999999999999999999999999999999.jpg?raw=true)


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



## 设置TextView内容滚动

1.设置android:scrollbars="vertical" 
```java

 <TextView
        android:layout_gravity="center"
        android:text="1、本卡不兑换现金；不能透支；可退
余额；不计息。 
2、网实行会员卡实名制，一人一卡，不得转借他人使用。 
3、本会员卡同时是开启会员专属储物柜的钥匙，卡号同储物柜号一致，请妥善保存，如有损坏、丢失，请迅速挂失，补卡、换卡时需交纳10 元工本费。 4、本卡只限本网使用. 
5、 网保留修改、变更、收回此卡的权利，并保留对此卡的最终解释权。
1、本卡不兑换现金；不能透支；可退
余额；不计息。 
2、网实行会员卡实名制，一人一卡，不得转借他人使用。 
3、本会员卡同时是开启会员专属储物柜的钥匙，卡号同储物柜号一致，请妥善保存，如有损坏、丢失，请迅速挂失，补卡、换卡时需交纳10 元工本费。 4、本卡只限本网使用. 
5、 网保留修改、变更、收回此卡的权利，并保留对此卡的最终解释权
1、本卡不兑换现金；不能透支；可退
余额；不计息。 
2、网实行会员卡实名制，一人一卡，不得转借他人使用。 
3、本会员卡同时是开启会员专属储物柜的钥匙，卡号同储物柜号一致，请妥善保存，如有损坏、丢失，请迅速挂失，补卡、换卡时需交纳10 元工本费。 4、本卡只限本网使用. 
5、 网保留修改、变更、收回此卡的权利，并保留对此卡的最终解释权"
        android:textSize="20sp"
        android:textColor="@color/colorPrimaryDark"
        android:id="@+id/tv"

          android:maxHeight="300px"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/title"
        android:scrollbars="vertical" />

```
2.设置setMovementMethod(ScrollingMovementMethod.getInstance())

```java
textView.setMovementMethod(ScrollingMovementMethod.getInstance());
```
效果
![这里写图片描述](https://github.com/ZhongXiaoHong/HappyTextView/blob/master/412431AC772F6A53A222CC6C3852C613.gif?raw=true)

