### 1.半透明边框
假设我们想给一个容器设置一层白色的背景和一道半透明白色的边框，body的背景会从它的半透明边框渗透上来。我们最开始的尝试可能是这样的：

	border:10px solid hsla(0,0%,100%,0.5);
	background: white;

并没有实现我们预期的效果。

默认情况下，背景会延伸到边框所在区域的下层。我们所做的事情并没有让body的背景从半透明白色边框处透上来，而是在半透明白色边框处透出了这个容器自己的纯白实色背景，这实际上得到的效果跟纯白实色的边框看起来完全一样。

解决方案：background-clip

	border:10px solid hsla(0,0%,100%,0.5);
	background: white;
	background-clip: padding-box;

----------

### 2.多重边框
#### box-shadow方案

	background: yellowgrenn;
	box-shadow: 0 0 0 10px #655,
				0 0 0 15px deeplink;

	background: yellowgrenn;
	box-shadow: 0 0 0 10px #655,
				0 0 0 15px deeplink,
				0 2px 5px 15px rgba(0,0,0,0.6);

注意：

1.投影的行为跟边框不完全一致，因为它不会影响布局，而且也不会受到box-sizing属性的影响。不过你还是可以通过内边距或外边距来额外模拟出边框所需要占据的空间。

2.他们不会响应鼠标事件，比如悬停或点击。如果这一点很重要，你可以给box-shadow属性加上inset关键字。

#### outline方案（描边）
	
	background: yellowgreen;
	border: 10px solid #655;
	outline: 5px solid deeplink;

另一个好处在于，你可以通过outline-offset属性来控制它跟元素边缘之间的距离，这个属性甚至可以接受负值，可以实现简单的缝边效果。

注意：

1.它只适用于双层“边框”的场景。
2.如果元素是圆角，他的描边可能还是直角。
3.描边可以不是矩形，请切记：最好在不同浏览器中完整地测试最终效果。

----------

### 3.灵活的背景定位
#### background-position的扩展语法方案
css3中的background-position属性得到扩展，它允许我们指定背景图片距离任意角的偏移量，只要我们在偏移量前面指定关键字。最后一步，我们还需要提供一个合适的回退方案。
	
	background:#58a url(code-pirate.png) bottom right no-repeat;
	background-position: right 20px bottom 10px;

#### background-origin方案
有一种情况极其常见，偏移量与容器的内边距一致。如果采用上面提到的background-position的扩展语法方案，是起作用的，但是代码不够DRY。

	padding: 10px;
	background:#58a url(code-pirate.png) bottom right no-repeat;
	background-origin: content-box;

#### calc()方案

	background:#58a url(code-pirate.png) bottom right no-repeat;
	background-position: calc(100%-20px) calc(100%-10px);

----------

### 4.边框内圆角

<img src="imgs/001.png">

用两个元素可以实现这个效果，但如果我们只需要一个元素，有没有办法可以只用一个元素达成同样的效果呢。

#### 解决方案
	<div class="box">I have a nice subtle inner rounding,don't I look pretty?</div>
	
	<style>
		width: 200px;
		margin: 20px;
		background: tan;
		border-radius: .8em;
		padding: 1em;
		box-shadow: 0 0 0 .4em #c00;
		outline: .6em solid #655;
	</style>

注意：阴影的扩张半径需要比描边的宽度值小，但它同时又要比(√2-1)r大(这里的r表示border-radius)。这意味着，如果描边的宽度比(√2-1)r小，那我们是不可能用这个方法达成该效果的。
（√2代表根号2，打不出来）

----------

### 5.条纹背景
#### 水平条纹

<img src="imgs/002.png" />
	
	width: 200px;
	height: 200px;
	background: linear-gradient(#f00 50%, yellow 0);
	background-size: 100% 30px;

<p></p>
<img src="imgs/003.png" />

	width: 200px;
	height: 200px;
	background: linear-gradient(#f00 33.3%, yellow 0, yellow 66.6%, yellowgreen 0);
	background-size: 100% 30px;

#### 垂直条纹

<img src="imgs/004.png" />

	width: 200px;
	height: 200px;
	background: linear-gradient(90deg, #f00 33.3%, yellow 0, yellow 66.6%, yellowgreen 0);
	background-size: 30px 100%;

#### 斜向条纹

<img src="imgs/005.png">
	
	width: 180px;
	height: 180px;
	background: linear-gradient(45deg, #f00 25%, yellowgreen 0, yellowgreen 50%, #f00 0,#f00 75%, yellowgreen 0);
	background-size: 30px 30px;

<img src="imgs/006.png">

但是这些条纹看起来要比我们前面制作的水平和垂直条纹细一些。（假设条纹的宽度是15px），改进如下：

<img src="imgs/007.png">

	width: 180px;
	height: 180px;
	background: linear-gradient(45deg, #f00 25%, yellowgreen 0, yellowgreen 50%, #f00 0,#f00 75%, yellowgreen 0);
	background-size: 42px 42px;

#### 更好的斜向条纹
利用 repeating-linear-gradient

	width: 180px;
	height: 180px;
	background: repeating-linear-gradient( 45deg, #79b 0, #79b 15px, #58a 0, #58a 30px);

#### 灵活的同色系条纹
	
	width: 180px;
	height: 180px;
	background: #58a;
	background-image: repeating-linear-gradient( 45deg, hsla(0,0%,100%,.1), hsla(0,0%,100%,.1) 15px, transparent 0, transparent 30px);

----------

### 6.复杂的背景图案
#### 网格

<img src="imgs/008.png"/>

	width: 195px;
	height: 195px;
	background: white;
	background-image: linear-gradient(90deg, rgba(200, 0, 0, 0.5) 50%, transparent 50%), linear-gradient(rgba(200, 0, 0, 0.5) 50%, transparent 50%);
	background-size: 30px 30px;

<br />
<img src="imgs/009.png"/>
	width: 195px;
	height: 195px;
	background: #58a;
	background-image: linear-gradient(90deg, white 1px, transparent 0),
					  linear-gradient(white 1px, transparent 0);
	background-size: 30px 30px;

#### 波点
<img src="imgs/010.png"/>
	width: 200px;
	height: 200px;
	background: #655;
	background-image: radial-gradient(tan 30%, transparent 0),
					  radial-gradient(tan 30%, transparent 0);	
	background-size: 30px 30px;
	background-position: 0 0, 15px 15px;

#### 棋盘
<img src="imgs/011.png"/>
	width: 200px;
	height: 200px;
	background: #eee;
	background-image: linear-gradient(45deg, #bbb 25%, transparent 0),
					  linear-gradient(45deg, transparent 75%, #bbb 0),
					  linear-gradient(45deg, #bbb 25%, transparent 0),
					  linear-gradient(45deg, transparent 75%, #bbb 0);	
	background-size: 30px 30px;
	background-position: 0 0, 15px 15px,
						 15px 15px, 0px 0px;

----------

### 7.伪随机背景
<img src="imgs/012.png"/>
	width: 100%;
	height: 200px;
	background: hsl(20, 40%, 90%);
	background-image: linear-gradient(90deg, #fb3 11px, transparent 0),
					  linear-gradient(90deg, #ab4 23px, transparent 0),
					  linear-gradient(90deg, #655 41px, transparent 0);	
	background-size: 41px 100%, 61px 100%, 83px 100%;

----------

### 8.连续的图像边框
<img src="imgs/013.png"/>
	width: 200px;
	padding: 1em;
	border: 1em solid transparent;
	background: linear-gradient(white, white),
				url(images/app_ios.png);
	background-size: cover;
	background-clip: padding-box, border-box;
	background-origin: border-box;

<br />
<img src="imgs/014.png"/>
	width: 200px;
	border: 1em solid transparent;
	padding: 1em;
	background: linear-gradient(white, white) padding-box,
				repeating-linear-gradient(-45deg, red 0, red 12.5%, transparent 0, transparent 25%, #58a 0, #58a 37.5%, transparent 0, transparent 50%) 0/5em 5em;

<br />
<img src="imgs/015.png"/>
	@keyframes ants {
		to{
			background-position: 100%
		}
	}
	
	.box{
		width: 200px;
		padding: 1em;
		border: 1px solid transparent;
		background: linear-gradient(white, white) padding-box, repeating-linear-gradient(-45deg, black 0, black 25%, white 0, white 50%) 0/0.6em 0.6em;
		animation: ants 12s linear infinite;
	}
----------
