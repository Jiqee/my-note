一、UI方面

    1、根据UI效果图，能用相对布局搞定的就用相对布局，可以减少多尺寸设备适配工作量。在用到线性布局的情况下，非必要情况慎用layout_weight属性。layout_weight属性会onMesure两次，影响绘制速度。
		
    2、坚持宽而浅原则：
    能少用布局嵌套就少用布局嵌套，方便提高布局解析速度。活用<include />或者<merge />，使用<include />会自动忽略<merge />这个层级。
		
    3、lint原则：
    使用compound drawables - 一个包含了ImageView与TextView的LinearLayout可以被当作一个compound drawable来处理。
    使用merge根框架 - 如果FramLayout仅仅是一个纯粹的（没有设置背景，间距等）布局根元素，我们可以使用merge标签来当作根标签。
    无用的分支 - 如果一个layout并没有任何子组件，那么可以被移除，这样可以提高效率。
    无用的父控件 - 如果一个layout只有子控件，没有兄弟控件，并且不是一个ScrollView或者根节点，而且没有设置背景，那么我们可以移除这个父控件，直接把子控件提升为父控件】
    深层次的layout - 尽量减少内嵌的层级，考虑使用更多平级的组件 RelativeLayout or GridLayout来提升布局性能，默认最大的深度是10。
    
    4、抽取公共布局或者控件，集成style属性。
    
    5、懒加载View 标记---viewstub
    ViewStub是一个轻量级的view，没有占有空间，没有花费draw的资源，也没有参与在任何一个layout的计算与绘制里面。创建它仅需要很少的系统资源，而且存留在View的层级也是个比较不花费资源的动作。每一个ViewStub简单的包含一个android:layout的属性来指定待创建的布局文件。载用方式：setVisibility(View.VISIBLE) 或者是调用 inflate()。
    
    6、针对listview来说，用viewholder设计模式可以有效减少频繁地findbyId操作，设置tag标记，方便查询。 
    
