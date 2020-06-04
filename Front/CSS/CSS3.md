## CSS3新特性

#### 1.2D变形

```css
<style>
		div{
			width: 200px;
			height: 200px;
			background-color: pink;
			/*transform: translate(100px); 水平移动100像素*/
			/*transform: translate(50%); transform移动的是自己的50%*/
			position: absolute;
			left: 50%;
			top: 50%;
			/*margin-left: -100px; 需要自己调不合适 */
			transform: translate(-50%,-50%);
		}
	</style>
```

#### 设置动画

##### 中心点

```css
		img{
			margin: 100px;
        transition: all 0.5s;
			/*transform-origin: bottom right;*/
		}
		img:hover{
			transform: rotate(720deg);
		}
```

##### 旋转

```css
div{
			width: 250px;
			color: #0ff0;
			height: 170px;
			border: 1px solid pink;
			margin: 200px auto;
			position: relative;
		}
		div img {
			width: 100%;
			height: 100%;
			position: absolute;
			top: 0;
			left: 0;
			transition: all 1s;
			transform-origin: left top; 
		}
		div:hover img:nth-child(1) {
			transform: rotate(60deg);
		}
		div:hover img:nth-child(2) {
			transform: rotate(120deg);
		}
		div:hover img:nth-child(3) {
			transform: rotate(180deg);
		}
		div:hover img:nth-child(4) {
			transform: rotate(240deg);
		}
		div:hover img:nth-child(5) {
			transform: rotate(300deg);
		}
```

##### x轴旋转

```css
		img{
			margin: 100px;
			transition: all 3s;
			transform-origin: left top; 
		}
		img:hover{
			transform: rotateX(360deg);
		}
```

##### Y轴旋转

```css
		body{
			perspective: 1000px;
		}
		img{
			margin: 100px;
			transition: all 5s;
			transform-origin: left;
		}
		img:hover {
/*			transform: rotateY(80deg);
			transform: rotateX(800deg);*/
			transform: rotate3d(100px,50px,100px);
		}
```

##### 两面反转

```css
div{
			width: 224px;
			height: 224px;
			margin: 100px auto;
			position: relative;
		}
		div img {
			position: absolute;
			top: 0;
			left: 0;
			transition: all 1s;
		}

		div img:first-child{
			z-index: 1;
			backface-visibility: hidden;/*
			不是正面就隐藏*/
		} 
		div:hover img{
			transform: rotateY(180deg);
		}
```

##### 动画

```css
div{
			width: 100px;
			height: 50px;
			background-color: pink;
			animation: go 2s ease 0s infinite reverse;
			/*animation:动画名称 动画时间 运动曲线  何时开始  播放次数  是否反方向;*/
		}
		/*@keyframes 动画名称{} 定义动画*/
		@keyframes go {
			/*from{
				transform: translateX(50px)
			}
			to{
				transform: translateX(600px);
			}*/
			0%{
				transform: translate3d(0,0,0);
			}
			25%{
				transform: translate3d(800px,0,0);
			}
			50%{
				transform: translate3d(800px,500px,0);
			}
			75%{
				transform: translate3d(0px,500px,0);
			}
			100%{
				transform: translate3d(0,0,0);
			}
		}
```

##### 小车移动+翻转

```
img{
			animation: car 5s infinite;
		}
		@keyframes car {
			0%{
				transform:translate3d(0,0,0);
			}
			50%{
				transform: translate3d(1000px,0,0);
			}
			51%{ /*车要掉头*/
				transform: translate3d(1000px,0,0) rotateY(180deg) ; 
			}
			99%{
				transform: translate3d(0,0,0) rotateY(180deg);
			}
		}
```

##### 无缝滚动

```css
*{
			margin: 0;
			padding: 0;
		}
		ul{
			list-style: none;
		}
		nav{
			width: 882px;
			height: 86px;
			border: 1px solid pink;
			margin: 100px auto;
						overflow: hidden;
		}
		nav li{
			float: left;
		}
		nav ul{
			width: 200%;
			animation: moving 5s linear infinite;/*匀速暂停*/

		}
		@keyframes moving{
			from{
				transform: translateX(-882px);
			}
			50%{
				transform: translateX(0) ; 
			}
			to{
				transform: translateX(882px);
			}
		}
		nav:hover ul{
			animation-play-state: paused;
			/*鼠标经过时暂停*/
		}
```

