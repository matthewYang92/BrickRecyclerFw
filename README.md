# BrickRecyclerView Framework

## 背景
基于RecyclerView封装的列表框架，快速实现多类型列表UI，注解编程，解放你的劳动力，最快只需要写一个View！

## Gradle依赖

Add this in your app build.gradle:

```java
dependencies {
    ...
    compile 'io.yang:brickfw-source:2.5.3'
    annotationProcessor 'io.yang:brickfw-compiler:2.5.3'
}
```

## 代码使用

使用方法：</br>
1：通过注解@BrickView(value = "type")定义itemView</br>
2: 构建List<BrickInfo> bricks列表
3：BrickRecyclerView.setBrickList(bricks)生成列表

简单点击事件通过注解@OnBrickItemClick, @OnBrickItemLongClick处理，用法与ButterKnife相似

详情见demo

### 初始化
```java
    public class App extends Application {
        @Override
        public void onCreate() {
            ....
            BrickFactory.init();
        }
    }
```

### BrickInfo数据结构
```java
    public class BrickInfo {

        private String mType;                   // 组件类型

        private Object mExtra;                  // 对应数据data，onBindViewHolder需要绑定的数据

        private int mColumns = 1;               //布局信息 列数

        private BrickPositionInfo positionInfo; //位置信息 加载到列表后赋值

        ....
    }
```

### 生成BrickRecyclerView列表
```java


List<BrickInfo> bricks = new ArrayList();
for (int i = 0; i < 5; i++) {
    BrickInfo info = new Brick();
    info.setType("image_and_text");
    info.setExtra(new ImageText(url, content)));
    bricks.add(info);
}
brickRecyclerView.setBrickList(bricks);

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

    @OnBrickItemClick("image_and_text")
    public void onClickImage(BrickInfo info, ImageTextView view) {
        ...
    }
```

### BrickRecyclerView的一些常用方法
```java
    /**
    布局相关
    **/
    setOrientation(int orientation); //设置列表方向 HORIZONTAL VERTICAL
    setStaggeredLayout(int columns); //设置列表为瀑布流布局
    setNormalLayout(Context context); //设置列表为普通布局 默认行数为1

    /**
    局部刷新相关
    **/
    updateRange(int pos, int itemCount);
    updateItem(int pos);
    updateRange(int pos, int itemCount, Object payload); //带payload,实现itemView内局部刷新
    updateItem(int pos, Object payload); //带payload,实现itemView内局部刷新

    /**
    动画相关
    **/
    setDefaultAnimator(boolean open); //是否开启默认动画
```

### 分割线的使用
```java
    View或Holder实现IDecoration接口
```


