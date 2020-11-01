### link与@import的区别
1. link是 HTML 方式， @import是 CSS 方式
2. link最大限度支持并行下载，@import过多嵌套导致串行下载，出现FOUC
3. link可以通过rel="alternate stylesheet"指定候选样式
4. 浏览器对link支持早于@import，可以使用@import对老浏览器隐藏样式
5. @import必须在样式规则之前，可以在 css 文件中引用其他文件
总体来说：link 优于@import

### display: none;与visibility: hidden;的区别
联系：它们都能让元素不可见
区别：
1. display:none;会让元素完全从渲染树中消失，渲染的时候不占据任何空间；visibility: hidden;不会让元素从渲染树消失，渲染时元素继续占据空间，只是内容不可见。
2. display: none;是非继承属性，子孙节点消失由于元素从渲染树消失造成，通过修改子孙节点属性无法显示；visibility: hidden;是继承属性，子孙节点由于继承了 hidden 而消失，通过设置 visibility: visible，可以让子孙节点显示。
3. 修改常规流中元素的 display 通常会造成文档重排。修改 visibility 属性只会造成本元素的重绘。
读屏器不会读取 display: none;元素内容；会读取 visibility: hidden;元素内容。

### BFC 
#### 触发条件或者说哪些元素会生成BFC：
满足下列条件之一就可触发BFC
- 根元素，即HTML元素
- float的值不为none
- overflow的值不为visible
- display的值为inline-block、table-cell、table-caption
- position的值为absolute或fixed
#### BFC布局规则：
1. 内部的Box会在垂直方向，一个接一个地放置。
2. Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠
3. 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
4. BFC的区域不会与float box重叠。
5. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
6. 计算BFC的高度时，浮动元素也参与计算
#### BFC有哪些作用：
1. 自适应两栏布局
2. 可以阻止元素被浮动元素覆盖
3. 可以包含浮动元素——清除内部浮动
4. 分属于不同的BFC时可以阻止margin重叠

### px、em和rem区别
- px像素（Pixel）绝对长度单位。像素px是相对于显示器屏幕分辨率而言的。
- em是相对长度单位。相对于当前对象内文本的字体尺寸。如当前对行内文本的字体尺寸未被人为设置，则相对于浏览器的默认字体尺寸。
- rem是CSS3新增的一个相对单位（root em，根em）。
  
### 移动端1px的问题
#### WWDC对iOS统给出的方案
在 WWDC大会上，给出来了1px方案，当写 0.5px的时候，就会显示一个物理像素宽度的 border，而不是一个css像素的 border。 所以在iOS下，你可以这样写。
``````
border:0.5px solid #E5E5E5
``````
- 优点：简单，没有副作用
- 缺点：支持iOS 8+，不支持安卓。后期安卓follow就好了。
#### 使用边框图片
``````
border: 1px solid transparent;
border-image: url('./../../image/96.jpg') 2 repeat;
``````
- 优点：没有副作用
- 缺点：border颜色变了就得重新制作图片；圆角会比较模糊。
#### 使用box-shadow实现
``````
box-shadow: 0  -1px 1px -1px #e5e5e5,   //上边线
            1px  0  1px -1px #e5e5e5,   //右边线
            0  1px  1px -1px #e5e5e5,   //下边线
            -1px 0  1px -1px #e5e5e5;   //左边线
``````
- 优点：使用简单，圆角也可以实现
- 缺点：模拟的实现方法，仔细看谁看不出来这是阴影不是边框。
#### 使用伪元素
1)一条border
```````
.setOnePx{
  position: relative;
}
.setOnePx:after{
    position: absolute;
    content: '';
    background-color: #e5e5e5;
    display: block;
    width: 100%;
    height: 1px; /*no*/
    transform: scale(1, 0.5);
    top: 0;
    left: 0;
}
```````
2)全部border
```````
.setBorderAll{
    position: relative;
}
.setBorderAll:after{
    content:" ";
    position:absolute;
    top: 0;
    left: 0;
    width: 200%;
    height: 200%;
    transform: scale(0.5);
    transform-origin: left top;
    box-sizing: border-box;
    border: 1px solid #E5E5E5;
    border-radius: 4px;
}
```````
- 优点：全机型兼容，实现了真正的1px，而且可以圆角。
- 缺点：暂用了after 伪元素，可能影响清除浮动。
#### 设置viewport的scale值
利用viewport+rem+js 实现
```````
<html>
  <head>
      <title>1px question</title>
      <meta http-equiv="Content-Type" content="text/html;charset=UTF-8">
      <meta name="viewport" id="WebViewport" content="initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no">
      <style>
          html {
              font-size: 1px;
          }
          * {
              padding: 0;
              margin: 0;
          }
          .top_b {
              border-bottom: 1px solid #E5E5E5;
          }

          .a,.b {
                      box-sizing: border-box;
              margin-top: 1rem;
              padding: 1rem;
              font-size: 1.4rem;
          }

          .a {
              width: 100%;
          }

          .b {
              background: #f5f5f5;
              width: 100%;
          }
      </style>
      <script>
          var viewport = document.querySelector("meta[name=viewport]");
          //下面是根据设备像素设置viewport
          if (window.devicePixelRatio == 1) {
              viewport.setAttribute('content', 'width=device-width,initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no');
          }
          if (window.devicePixelRatio == 2) {
              viewport.setAttribute('content', 'width=device-width,initial-scale=0.5, maximum-scale=0.5, minimum-scale=0.5, user-scalable=no');
          }
          if (window.devicePixelRatio == 3) {
              viewport.setAttribute('content', 'width=device-width,initial-scale=0.3333333333333333, maximum-scale=0.3333333333333333, minimum-scale=0.3333333333333333, user-scalable=no');
          }
          var docEl = document.documentElement;
          var fontsize = 32* (docEl.clientWidth / 750) + 'px';
          docEl.style.fontSize = fontsize;
      </script>
  </head>
  <body>
      <div class="top_b a">下面的底边宽度是虚拟1像素的</div>
      <div class="b">上面的边框宽度是虚拟1像素的</div>
  </body>
</html>
```````
- 优点：全机型兼容，直接写1px不能再方便
- 缺点：适用于新的项目，老项目可能改动大