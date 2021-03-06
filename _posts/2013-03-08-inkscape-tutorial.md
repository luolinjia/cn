---
layout: post
title: Inkscape基础
categories:
- Book
- Study
tags:
- linux
- inkscape
---

> 来自[官网](http://inkscape.org/doc/basic/tutorial-basic.zh_CN.html)转载  


# 平移画布  

平移画布(卷屏)的方法有很多种。使用Ctrl+arrow键可以用键盘卷屏。(你可以尝试这些按键来卷动本文档。) 也可以通过鼠标中键来拖动画布，或者使用屏幕边缘的滚动条(使用Ctrl+B来显示或隐藏滚动条)。鼠标滚轮wheel可以上下卷动画布，按住Shift键，配合滚轮则可以水平卷动。  


# 放大与缩小  

最简单的缩放操作是通过-和+（或=）键。也可以通过Ctrl+middle click 或 Ctrl+right click来放大，Shift+middle click 或 Shift+right click 来缩小画布。也可以用Ctrl键配合鼠标滚轮来缩放。或者在窗口右下角的缩放输入框中输入一个准确的百分比数值。在工具栏中也有缩放按钮，可以缩放到用户选定的区域（对象）。  

Inkscape还会记录当前工作会话中使用的缩放历史，按`键回到上一次的缩放比例，Shift+`键来恢复撤销的缩放比例。  


# Inkscape工具列  

Inkscape中的绘图和修改工具集中在左侧的竖直工具列中。在窗口的上方，菜单下面是命令栏(Commands bar)，提供了通用的一些控制命令，下面的工具控制栏(Tool Controls bar)则跟具体的绘图工具有关。窗口底部的状态栏(status bar)则实时显示一些操作提示和信息。  

很多操作都有对应的快捷键,在帮助菜单中选择 鼠标与快捷键(Help > Keys and Mouse)获取详细的说明。  


# 创建和管理文档  

选择菜单文件File > 新建New，或使用快捷键Ctrl+N新建文档。选择菜单文件File > 打开Open (Ctrl+O)打开已有文档。选择菜单文件File > 保存Save (Ctrl+S)来保存文件。或者选择菜单文件File > 另存为Save As (Shift+Ctrl+S)将当前文件以不同的文件名保存。（Inkscape可能有时不够稳定，切记经常保存！）  

Inkscape使用SVG(Scalable Vector Graphics可缩放矢量图形)文件格式。 SVG是一种被各种绘图软件广泛支持的开放文件标准。SVG文件是基于XML的，可以用任何文本和XML编辑器来编辑(Inkscape不属于这种文本编辑器)。除SVG外，Inkscape也可以导入和导出其它一些文件格式(EPS,PNG等)。  

Inkscape为每个文档打开一个独立的窗口。你可以用操作系统中的窗口管理器来在各个窗口间切换(例如Alt+Tab键)，也可以使用Inkscape中内置的快捷键Ctrl+Tab在文档间循环切换。(现在可以新建一个文档，尝试在本文档和新文档间切换。) 注意：Inkscape将这些窗口看成类似于浏览器的标签页Tabs，即Ctrl+Tab只对同一个进程中的文档有效。如果你从文件管理器或Inkscape图标打开多个进程，这个快捷键将无效。  


# 创建形状  

下面我们开始创建一些很漂亮的图形！在工具列中选择矩形工具(Rectangle)(快捷键F4),在(本文档或新文档的)绘图区中点击、拖动：  

![inkscape1](https://ws3.sinaimg.cn/large/006tKfTcly1fiscbwrqd5j30e602rjr5.jpg)  

As you can see, default rectangles come up blue, with a black stroke (outline), and fully opaque. We'll see how to change that below. With other tools, you can also create ellipses, stars, and spirals:  

![inkscape2](http://i.imgur.com/J6V8f1n.png)  

这些工具统称为形状工具shape tools。 新创建的每一个形状上都有一个或更多四边形的控制器(handles)； 试一下拖动这些控制器会产生什么样的效果。在工具控制栏中也可以对形状进行修改。工具控制栏只对当前选中的形状有效(显示出四边形控制器的)，但同时也会成为当前形状工具的缺省参数，影响下次创建的图形。  

按键Ctrl+Z可以撤销(undo)上一次操作。(如果你又改变注意了，可以用Shift+Ctrl+Z来恢复(redo)撤销的操作。)  


# 移动、缩放和旋转  

Inkscape中最常用的工具是拾取器(Selector)，位于工具列的顶端(箭头形状)，对应快捷键F1 或者 空格(Space)。现在你可以选择当前画布上的任何对象。请点击下面的矩形。  

![inkscape3](https://ws1.sinaimg.cn/large/006tKfTcly1fisdygyqdfj302c0280aw.jpg)  

可以看到，选择对象的周围出现八个带箭头的控制器。下面你可以：  

- 通过拖动来移动对象。(按下Ctrl来进行水平或竖直移动。)
- 通过拖动任意的控制器来缩放。(按下Ctrl以保持原始的宽度-高度比例。)  

再次在矩形上单击，控制器会发生变化，现在你可以：  

- 拖动对象角落上的控制器来旋转。(按下Ctrl以保持旋转的角度为15度的整数倍。)
- 拖动中间的控制器来扭曲(倾斜)对象。按下Ctrl以保持扭曲的角度为15度的整数倍。)  

在选择状态，也可以在工具控制栏(画布的上方)的输入框中输入数字，精确地控制对象的位置坐标(x,y)和尺寸(宽度W,高度H)。  


# 通过键盘变换  

Inkscape区别于大多数其它矢量绘图软件的一个特征是键盘操作的便捷性。几乎所有的命令都可以通过键盘实现，变换操作也不例外。  

你可以用键盘来对编辑对象进行移动(arrow 光标键)，缩放(< 和 > 键)，以及旋转([ 和 ] 键)。缺省情况下，每次移动和缩放2px，按下Shift键时扩大为10倍。Ctrl+> 和 Ctrl+< 对应的缩放比例分别为原始的200%和50%。缺省每次旋转 15 度，通过Ctrl键，每次可以旋转90度。  

可能更有用的是像素级别的变换(pixel-size transformations), 实现的方法是，在上面的快捷键基础上配合Alt键。例如Alt+arrows可以在当前的页面视图层次上每次移动一个像素(这里是指一个屏幕像素的距离， 而不是SVG中的与视图缩放级别无关的长度单位px)。这意味着，如果放大视图，Alt+arrow移动一个像素的绝对距离将缩短。这样，通过缩放视图，就可以任意控制对象的定位精度。  

与此类似，Alt+> 和 Alt+< 将选择对象每次缩放一个像素， Alt+[ 和 Alt+] 旋转对象时，距离旋转中心最远的位置每次移动一个像素。  

注意：在Linux操作系统中，这些组合键可能在窗口管理器中被指定了其它的用途，执行上述操作时可能不能获得预期的结果。解决的方法当然是相应地修改窗口管理器的配置。  


# 多选  

通过Shift+click，可以连续选择多个绘图对象，或者，用鼠标左键拖出一个框来选中框内所有对象，这个也称为弹性区选(rubberband selection)。(从空白处开始拖动时将创建弹性选区，如果在拖动之前先按下Shift，则总是创建弹性选区。) 请尝试选择下面的三个形状：  

![inkscape4](https://ws1.sinaimg.cn/large/006tKfTcly1fisdyigl2aj307b01yt8k.jpg)  

你可以使用弹性选区选择下面两个椭圆，但不包括矩形：  

![inkscape5](https://ws1.sinaimg.cn/large/006tKfTcly1fiscia2w77j308h02a743.jpg)  

被选择的对象上会出现一个选择标识(selection cue)，默认情况下是一个虚线矩形框，它可以标识出哪些对象被选中，哪些没有选中。例如，同时选中两个椭圆和矩形时，如果没有矩形标识框，椭圆的选中与否就难以判断。  

在已经选择的对象上Shift+click可以取消选择。选中上面的三个对象，然后用Shift+click取消对两个椭圆的选择，只选中矩形。  

按Esc取消所有选择，Ctrl+A选择当前图层上的所有对象(如果没有定义图层，则等价于选中文档中的所有绘图对象)。  


# 群组  

若干个绘图对象可以组合为一个群组group。群组可以像普通绘图对象一样进行移动或变换。下图中，左边的三个图形是互相独立的，而右边的三个图形是组合在一起的。试着拖动这个群组看看。  

![inkscape6](https://ws2.sinaimg.cn/large/006tKfTcly1fiscjdjretj30dy03njrg.jpg)  

选择一个或多个对象后，按Ctrl+G可以将它们组合在一起。选中一个或多个群组后，按Ctrl+U可以解散组合。群组也可以再次组合，并且群组的嵌套层数没有限制。不过Ctrl+U只能打开最顶层的群组，对于嵌套群组需要多次Ctrl+U才能完全打散组合。  

实际上你可以直接修改群组内的对象而不用取消组合。使用Ctrl+click就可以单独选中群组内的一个对象，进行编辑；使用Shift+Ctrl+click则可以选中群组内或群组外的多个对象。不需要解散群组，请试着对上图右面群组中的形状进行单独的移动、变换。然后再选中群组，可以看到这种组合关系仍然存在。  


# 填充与轮廓  

Inkscape中的许多功能都借助于对话框的形式。为绘图添加一些色彩的最简单的方法是打开视图View菜单中的调色板Swatches对话框(快捷键Shift+Ctrl+W)，然后为对象选择一种(填充)颜色。  

更强大的工具是对象Object菜单中的填充与轮廓对话框 (或者按 Shift+Ctrl+F)。选中下面的形状，然后打开填充与轮廓对话框。  

![inkscape7](https://ws4.sinaimg.cn/large/006tKfTcly1fisektkh7ej307g01wglf.jpg)  

这个对话框中有三个标签面板：填充Fill、轮廓色彩(Stroke paint)和轮廓样式(Stroke style)。填充属性可以修改对象的内部fill 。下面的按钮可以设置填充的类型，包括不填充(图标X)，单色flat color填充，以及渐变(gradients,线性或圆周)填充。对于上面的椭圆，单色填充的按钮是激活的。  

这些按钮的下面，是色彩拾取器color pickers，有四种不同的方式：RGB, CMYK, HSL,色盘Wheel。可能最方便是通过色盘来选择，旋转其中的三角形来选择色调，在三角形内可以拾取不同的明暗度。四中拾取方式中都包含一个滑动条来设置对象的透明度(opacity)，即alpha值。  

选择不同的对象时，色彩拾取器总是自动更新，对应当前对象的填充和笔廓。(选择多个对象时，将显示色彩的平均值) 。在下面的例子上做一下练习，或者自己创建图形：  

![inkscape8](http://i.imgur.com/sEzW0gn.png)  

在轮廓色彩Stroke paint标签中，可以删除轮廓线stroke，也可以任意为其指定颜色和透明度：  

![inkscape9](https://ws4.sinaimg.cn/large/006tKfTcly1fise0nr5n2j30ec01xmx8.jpg)  

最后一个标签面板，轮廓样式(Stroke style)中，可以设置轮廓的宽度以及其它参数：  

![inkscape10](http://i.imgur.com/z7Ho7GJ.png)  

最后，除了单色填充之外，可以选择梯度(gradients)模式来填充图形内部和轮廓：  

![inkscape11](http://i.imgur.com/e6MfWc7.png)  

当从单色填充切换到梯度填充时，颜色仍然是前面单色填充时的颜色，不同的是透明度从不透明渐变到完全透明。 选择工具列中的渐变工具(Gradient tool,Ctrl+F1)，对象上将会显示出(用线连接在一起的)渐变控制器，拖动渐变控制器(gradient handles)，可以改变色彩梯度的方向和范围。选中某个控制器时(该控制器呈现蓝色)，可以在填充和轮廓中为该控制器单独设置色彩，实现从一种颜色到另一种颜色的渐变。  

还有一种改变对象色彩的简便方法是使用滴管工具(Dropper tool,F7)。选择对象后，再选择该工具，然后可以在绘图中单击click任意拾取色彩，这种色彩将自动指定给被选择对象的填充属性(使用Shift+click  


# 再制、对齐和分布  

一个常用的操作是生成对象的一个副本，即再制duplicating(Ctrl+D)。新生成的副本与原对象重合(垂直于纸面方向)，并且已经被选中。可以用鼠标或光标键把它移走。想练习一下？请将下面一行用这个黑方块填满：  


移动后，新的方块的位置难免些不够整齐，这时对齐对话框(Align dialog,Ctrl+Shift+A)就派上用场了。选择所有的方块(Shift+click或者拖出一个弹性选区)，打开这个对话框，选择“中心水平对齐(Center on horizontal axis)”，再选择“水平等间距分布(Make horizontal gaps between objects equal)”，这些方块的位置就很整齐匀称了。下面是一些利用对齐和分布工具生成的图案：  

![inkscape12](http://i.imgur.com/yQkPf4u.png)  


# 叠放次序Z-order  

z-order指的是绘图中对象的叠放次序，例如，某个对象在最上层，盖住了其它的对象。对象(Object)菜单中的两个命令，置顶(Raise to Top,对应Home键)与置底(Lower to Botton,End键)，将使所选对象置于当前图层叠放次序(Z方向)的顶部或底部。另外两个命令上升(Raise，PgUp键)与下降(Lower，PgDn键)，将使被选择对象上升或下将一个位次，例如，可以将当前对象移动到它上面一个图形的上面。(如果所选对象与其它对象都不重叠，上移和下移分别等同于置顶和置底。)  

可以在下面的图形上练习改变叠放次序，让最左边的椭圆位于最上层，而最右边的椭圆位于最下层：  

![inkscape13](https://ws3.sinaimg.cn/large/006tKfTcly1fise2uhp45j307q02xq2w.jpg)  

选择叠放的对象时，一个很方便的快捷键是Tab。如果没有选择任何对象，按Tab将会选择最底层的对象；有对象被选中时，将选择其上的对象。Shift+Tab的选择方向则相反，从最顶层开始，往底层逐次选择。默认的叠放次序与图形创建的次序是一样的，所以没有选择对象时，Shift+Tab总是选择刚创建的图形。在上面的叠放椭圆中可以练习一下Tab和Shift+Tab的选择。  


# 选择下面的对象并移动  

如果一个对象完全被另一个对象盖住了，该怎么选择呢？如果上面的图形是透明的，你虽然可以看到下面的对象，但点击时选中的却是上面的图形。  

这就是Alt+click要干的活。首先在上面的图形上Alt+click，这将选中它，然后在相同的位置上再次Alt+click，这次将选择该位置处，顶层图形下面的对象。对于多层叠放，多次Alt+click实现从顶层到底层的循环选择。  

[如果你在Linux系统中工作，Alt+click可能不会像前面描述的那样工作，反而可能会移动整个Inkscape窗口。这是因为窗口管理器为Alt+click指定了其它用途。 你需要找到窗口管理器的相应配置，把其中的这个快捷键关掉，或选择其它的组合键。]  

选中了被盖住的图形，你又可以做什么呢？可以用光标键移动，可以用鼠标拖动控制器。但是，如果拖动整个对象，则会重新选择顶部的图形(这是点击-拖动的工作模式，总是选中顶部的对象然后拖动)。要让Inkscape拖动当前选择的对象，而不是顶部的对象，需要借助于Alt+drag，这将拖动当前选择的对象，而不论你的鼠标在哪里。  

请用Alt+click和Alt+drag选择并拖动绿色透明矩形下的棕色形状：  

![inkscape14](https://ws2.sinaimg.cn/large/006tKfTcly1fisep3lc24j309k035jr9.jpg)  


# 总结  

好了，最后做一下小结。对于Inkscape，这仅仅是开始，但靠这几招，你已经可以做一些简单但不失实用的图形了。更高级的复杂操作，请参照Help > Tutorials中的其它教程。  
   