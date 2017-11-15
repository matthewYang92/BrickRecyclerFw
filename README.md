# BrickRecyclerView Framework

## 背景
基于RecyclerView封装的列表框架，快速实现多类型列表UI，注解编程，解放你的劳动力，最快只需要写一个View！

## Gradle依赖

Add this in your app build.gradle:

```java
dependencies {
    ...
    compile 'io.yang:brickfw-source:2.4.4'
    annotationProcessor 'io.yang:brickfw-compiler:2.4.4'
}
```

## 代码使用

使用方法：</br>
1：通过注解@BrickView(value = "type")定义itemView</br>
2：BrickRecyclerView.addData("type", data)生成列表

简单点击事件通过注解@OnBrickItemClick, @OnBrickItemLongClick处理，用法与ButterKnife相似

更多用法详见demo

### 初始化
```java
    @BrickInit
    public class App extends Application {
        @Override
        public void onCreate() {
            ....
            BrickFactory.init(getClass());
        }
    }
```

### 生成BrickRecyclerView列表
```java

for (int i = 0; i < 5; i++) {
    brickRecyclerView.addData("image_and_text", new ImageText(url, content));
}

@BrickView("image_and_text")
public class ImageTextView extends FrameLayout {

    private ImageView image;
    private TextView text;

    public ImageTextView(Context context) {
        super(context);
        init(context);
    }

    public ImageTextView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
        init(context);
    }

    private void init(Context context) {
        View root = inflate(context, R.layout.view_image_text, this);
        image = (ImageView) root.findViewById(R.id.image);
        text = (TextView) root.findViewById(R.id.text);
    }

    @SetBrickData("image_and_text")
    public void setData(ImageText imageText) {
        Glide.with(image.getContext()).load(imageText.getUrl()).into(image);
        text.setText(imageText.getContent());
    }
}
```

### 关于事件响应
```java
    //设置事件处理者
    brickRecyclerView.setEventHandler(this);

    @OnBrickItemClick("simple_image")
    public void onClickImage(BrickInfo info, ImageView imageView) {
        ...
    }

    //自定义事件
    @OnBrickEvent(value = "image_and_text", eventType = 0)
    public void handleImageTextClickEvent(BrickInfo info, Object... params) {
        ...
    }
```

### 关于模块数据类BrickInfo
```java
    String mType;           // 模块类型
    Object mExtra;          // 本地字段，用于存储不同模块设置的个性数据
    int mColumns = 1;      //此模块的占位标识，默认为1，即一行一个模块
```

### BrickRecyclerView的一些常用方法
```java
    setOrientation(int orientation); //设置列表方向 HORIZONTAL VERTICAL
    setStaggeredLayout(int columns); //设置列表为瀑布流布局
    setNormalLayout(Context context); //设置列表为普通布局 默认行数为1
```

### 分割线的使用
```java
    View或Holder实现IDecoration接口
```


