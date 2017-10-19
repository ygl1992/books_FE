### 1.单侧投影
#### 单侧投影
<img src="imgs/001.png">

	box-shadow: 0 5px 4px -4px black;

#### 邻边投影
<img src="imgs/002.png">

	box-shadow: 3px 3px 6px -3px black;

#### 双侧投影
<img src="imgs/003.png">
	
	box-shadow: 5px 0 5px -5px #000,
				-5px 0 5px -5px #000;

----------

### 2.不规则投影
解决方案：滤镜效果

	filter: drop-shadow(2px 2px 10px rgba(0,0,0,.5));

----------

### 3.染色效果
<img src="imgs/004.png">

#### 基于滤镜的方案

	img{
		transition: 0.5s filter;
		filter: sepia(1) saturate(4) hue-rotate(295deg);
	}
	img:hover{
		filter: none;
	}

#### 基于混合模式的方案
一、
	
	<a href="#"><img src="images/app_ios.png" alt=""></a>

	<style type="text/css">
		a{
			display: inline-block;
			background: hsl(335, 100%, 50%);
		}
		img{
			vertical-align: middle;
			mix-blend-mode: luminosity;
		}
	</style>
	
二、（这个方法我没试成功）

	<div class="tinted-image" style="background-image:url(tiger.jpg)"></div>
	
	.tinted-image {
		width: 640px; height: 440px;
		background-size: cover;
		background-color: hsl(335, 100%, 50%);
		background-blend-mode: luminosity;
		transition: .5s background-color;
	}
	.tinted-image:hover {
		background-color: transparent;
	}

----------

### 4.毛玻璃效果
<img src="imgs/005.png">

	body, main::before {
		background: url("tiger.jpg") 0 / cover fixed;
	}
	main {
		position: relative;
		background: hsla(0,0%,100%,.3);
		overflow: hidden;
	}
	main::before {
		content: '';
		position: absolute;
		top: 0; right: 0; bottom: 0; left: 0;
		filter: blur(20px);
		margin: -30px;
	}

----------

### 5.折角效果
#### 45°折角的解决方案
	
	width: 200px;
	height: 200px;
	background: #58a;
	background: linear-gradient(to left bottom, transparent 50%, rgba(0,0,0,0.4) 0) no-repeat 100% 0/2em 2em,
			    linear-gradient(-135deg, transparent 1.5em, #58a 0);

#### 其他角度的解决方案（圆角，渐变，投影可以不要）
	.box{
		position: relative;
		width: 200px;
		height: 200px;
		background: #58a;
		background: linear-gradient(-150deg, transparent 1.5em, #58a 0);
		border-radius: 0.5em;
	}
	.box:before{
		content: "";
		position: absolute;
		top: 0;
		right: 0;
		width: 1.73em;
		height: 3em;
		background: linear-gradient(to left bottom, transparent 50%, rgba(0,0,0,0.2) 0, rgba(0,0,0,-.4)) no-repeat 100% 0;
		transform: translateY(-1.3em) rotate(-30deg);
		transform-origin: bottom right;
		border-bottom-left-radius: inherit;
		box-shadow: -0.2em 0.2em 0.3em -0.1em rgba(0,0,0,0.15);
	}