**Move Animation:**

![Move And Refactor](https://github.com/seedante/UIView-Refacotr-Destruct-Animation/blob/master/Move%20and%20Refactor.gif)

**All Actions Animation:**

![All Actions](https://github.com/seedante/UIView-Refacotr-Destruct-Animation/blob/master/All%20Actions.5.1M.gif)

## UIView Extension for Refactor and Destruct Animation

A alternative for move animation and disappear animation. At the begining, I just want to create a image display animation after download it; after it's finished, I find it's not good for the scene, now this. There are many things you can do with the algorithm of adjusting view pieces.

I use it in UICollectionView insert/delete/move item, here is the [repo](https://github.com/seedante/CollectionViewAnimation.git). 

###Feature

Both refactor animation and destruct animation support:

1. Three directions: Horizontal, Vertical, Diagonal(from top left to bottom right).
2. Custom animation time: a little weird. Is there any animation not support this?
3. Custom piece size: set ratio relative to source view, use to width and height both.

Only refactor animation support:

1. Custom shining color: if nil, no shining effect.
2. Custom the area where all pieces appear: if nil, pieces all appear from 2X frame of view.

###Defect

Not support Auto Layout directly.

###Installation and Usage

Drag **UIViewRefactorAndDestructExtension.swift** file into your project.

Refactor API:

	func refactor()
	/**
	Refactoring a view in place use default parameter value.
	*/

	func refactorWithNewFrame(destinationFrame: CGRect?, piecesRegion jumpRect: CGRect?, shiningColor: UIColor?, direction: SDERefactorDirection = .Horizontal, refactorTime animationTime: NSTimeInterval = 0.6, pieceRatio ratio: CGFloat = 0.04, enableBigRegion: Bool = false)
	/**
     - parameter destinationFrame: the frame you want to change to. If you not specify this parameter, it will refactor self. I recommend that you change this from CGRect? to CGRect when you use.
     - parameter jumpRect:        the area where all pieces appear, if nil, is 2X frame of the view.
     - parameter shiningColor:    if you specify this parameter, add light like electric welding.
     - parameter direction:       the direction of animation
     - parameter animationTime:   the total time of animation
     - parameter ratio:           the ratio which piece to view, here the ratio is used on width and height both.
     - parameter enableBigRegion: you will get 2X or 4X size of general piece if you enable it. I love this, and it can reduce the time of animation. I recommend just enable it if the view is big enough.
    */


Thanks to default parameter values in swift, you can get a refactor animation and just need to specify three parameter at most. You can ignore any default value parameter and don't need to keep them in old order.

	theView.refactor()
	theView.refactorWithNewFrame(nil, piecesRegion: nil, shiningColor: nil)
	theView.refactorWithNewFrame(newFrame, piecesRegion: nil, shiningColor: nil, refactorTime: 1.0, enableBigRegion:false, partionRatio: 0.02)
	
Destruct API:

Similar to refactor API. Only three parameters to custom.

	func destruct()
	/**
	 Add a destruct animation on with default paprameter values and remove it from its superview.
	*/
	
	func destructWithDirection(direction: SDERefactorDirection = .Diagonal, animationTime: NSTimeInterval = 0.5, pieceRatio ratio: CGFloat = 0.05)
	/**
      Add a destruct animation on view and remove it from its superview.
      - parameter direction:     animation direction
      - parameter animationTime: animation time
      - parameter ratio:         piece ratio to view. It apply to width and height both.
     */

SampleCode:

	theView.destruct()
	theView.destructWithDirection(.Diagonal, animationTime: 0.5, partionRatio: 0.05)
	theView.destructWithDirection(.Diagonal,partionRatio: 0.02)
	
Note: In destruct, the view is removed from its superview, if you don't want this, remember modify the code in the `windUp:` method.
	
## UIView 扩展：重组和分解动画
移动和消失动画的替代选择。这个动画最初只是用来图片下载完之后来个特效的，后来发现这个动画不是很合适。想做成科幻片里的传送特效那样子，还是偏得有点远了。重组动画本身可玩性很大，你只需要调整下碎片的出现时间就能玩出很多花样来。

UICollectionView 里新添加 cell 时非常适合使用重组动画，后来为了删除动画在又添加了分解动画，[介绍博客在这里](http://www.jianshu.com/p/4323c54ad643)，[repo 在这里](https://github.com/seedante/CollectionViewAnimation.git)。	
###特性：
重组和分解动画都支持：

1. 三种方向：水平，垂直，对角线(从左上角到右下角)。
2. 自定义动画时间：好像没哪个动画不支持的吧。
3. 自定义碎片大小：通过设定 ratio 参数来指定碎片相对于源视图的比例。长宽比一样，你可以自己改动。

只有重组动画支持：

1. 自定义闪光颜色： 如果没有指定，就没有五毛闪光特效。
2. 自定义碎片出现的区域：没有指定的话，默认碎片在当前位置的两倍大小区域内出现。

###缺陷：
重组动画不支持 Auto Layout，直接设置 frame 会出现动画完成后回到原地的情况，毕竟直接设置 frame 和 Auto Layout 是有冲突的。

###安装和使用

直接把**UIViewRefactorAndDestructExtension.swift**文件丢进你的项目里，这样所有的 UIView 类都可以使用重组和分解动画了。参数的名字基本是我自己的口味，有些可能不明白，可以去源码里看看注释。函数原型说明我就不重复了。

Refactor API:

得益于 Swift 带来的默认值参数特性，你可以至多指定三个参数就可以使用重组动画了。本来这样的配置一般是通过属性来搞定，而在扩展里一般通过关联对象来实现，但是，现在有7个配置参数，那写起来叫一个感人，而且也实在太难看了。幸好 Swift 从其他语言中吸收了默认值参数这个特性， 对于喜欢用属性来进行配置的人来说就有点不方便了，你可以自己修改文件去实现你想要的配置参数。有了默认值参数，在调用方法时不需要写上全部的参数了，在 Xcode 里，你能看到后面两个方法的提示。而且默认值参数的顺序不一定按照原型的顺序，在默认值参数与其他类型的参数保持整体的相对位置的情况下，你可以任意调整默认值参数之间的顺序，而且个数也不一定与原型相同。

	theView.refactor()
	theView.refactorWithNewFrame(nil, piecesRegion: nil, shiningColor: nil)
	theView.refactorWithNewFrame(newFrame, piecesRegion: nil, shiningColor: nil, direction: .Horizontal, refactorTime: 0.6, partionRatio: 0.05, enableBigRegion:false)
	
Destruct API:

和上面的类似，只有三个参数可以配置。

	theView.destruct()
	theView.destructWithDirection(.Diagonal, animationTime: 0.5, pieceRatio: 0.05)

注意：我的实现里，分解动画的最后把视图自身从它的父视图里移除了，如果你不需要这样做，记得修改`windUp:`方法。
