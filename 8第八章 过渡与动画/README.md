#### 1.缓动效果
> 主要用到cubic-bezier

#### 2.逐帧动画
<img src="imgs/003.png" />

	@keyframes loader {
		to { background-position: -800px 0; }
	}
	.loader {
		width: 100px; height: 100px;
		background: url(img/loader.png) 0 0;
		animation: loader 1s infinite steps(8);
		
		/* 把文本隐藏起来 */
		text-indent: 200%;
		white-space: nowrap;
		overflow: hidden;
	}

#### 3.闪烁效果


#### 4.打字效果
<img src="imgs/001.png" style="border: 1px solid #ccc" />

> 1.它不适用于多行文本
> 2.汉字好像也不行
> 3.英文字母好像要设置字体是宋体比较合适

	<h1>CSS is awesome!</h1>
	
	<style type="text/css">
		@keyframes typing{
			from{
				width: 0;
			}
		}
		@keyframes caret{
			50%{
				border-color: transparent;
			}
		}
		h1{
			font-family: '宋体';
			width: 15ch;
			font-size: 16px;
			white-space: nowrap;
			border-right: 0.05em solid;
			overflow: hidden;
			animation: typing 8s steps(15),
					   caret 1s steps(1) infinite;
		}
	</style>

#### 5.状态平滑的动画
<img src="imgs/002.png" />

> 主要用到animation-play-state这个属性

	<div class="panoramic"></div>
	
	<style type="text/css">
		@keyframes panoramic{
			to{
				background-position: 100% 0;
			}
		}
		.panoramic{
			width: 200px;
			height: 456px;
			background: url('images/banner001.png');
			background-size: auto 100%;
			animation: panoramic 10s linear infinite alternate;
			animation-play-state: paused;
		}
		.panoramic:hover{
			animation-play-state: running;
		}
	</style>

#### 6.沿环形路径平移的动画
