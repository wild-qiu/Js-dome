# Js-dome
基础案例练习
<!DOCTYPE html>
<html lang="en">
<head>
 <meta charset="UTF-8">
 <title>焦点图和轮播</title>
 <style>
  *{ 
   margin: 0;
   padding: 0; 
  }
  a{
   text-decoration: none;
  }
  body { 
   padding: 20px;
  }
  #container { 
   width: 600px; /*这里600x400是图片的宽高*/
   height: 400px; 
   border: 3px solid #333; 
   overflow: hidden; /*隐藏溢出的图片，因为图片左浮动，总宽度为4200*/
   position: relative;
  }
  #list { 
   width:4200px; /*这里设置7张图片总宽度*/
   height: 400px; 
   position: absolute; /*基于父容器container进行定位*/
   z-index: 1;
  }
  #list img { 
   width: 600px; /*这里600x400是图片的宽高*/
   height: 400px; 
   float: left;
  }
  #buttons { 
   position: absolute; 
   height: 10px; 
   width: 100px; 
   z-index: 2; /*按钮在图片的上面*/
   bottom: 20px; 
   left: 250px;
  }
  #buttons span { 
   cursor: pointer; 
   float: left; 
   border: 1px solid #fff; 
   width: 10px; 
   height: 10px; 
   border-radius: 50%; 
   background: #333; 
   margin-right: 5px;
  }
  #buttons .on { 
   background: orangered; /*选中的按钮样式*/
  }
  .arrow { 
   cursor: pointer; 
   display: none; /*左右切换按钮默认先隐藏*/
   line-height: 39px; 
   text-align: center; 
   font-size: 36px; 
   font-weight: bold; 
   width: 40px; 
   height: 40px; 
   position: absolute; 
   z-index: 2; 
   top: 180px; 
   background-color: RGBA(0,0,0,.3); 
   color: #fff;
  }
  .arrow:hover { 
   background-color: RGBA(0,0,0,.7);
  }
  #container:hover .arrow { 
   display: block; /*当鼠标放上去容器上面就显示左右切换按钮*/
  }
  #prev { 
   left: 20px;
  }
  #next { 
   right: 20px;
  }
 </style>
 <script>
		window.onload = function () {
		var container = document.getElementById('container');
		var list = document.getElementById('list');
		var buttons = document.getElementById('buttons').getElementsByTagName('span');
		var prev = document.getElementById('prev');
		var next = document.getElementById('next');
		var index = 1; //用于索引当前按钮
		var len = 5; //图片的数量
		var animated = false; //用于判断切换是否进行
		var interval = 3000; //自动播放定时器秒数，这里是3秒
		var timer; //定时器
		function animate (offset) {
		animated = true; //切换进行中
		var time = 300; //位移总时间
		var inteval = 10; //位移间隔时间
		var speed = offset/(time/inteval); //每次位移量
		var left = parseInt(list.style.left) + offset; //目标值
		var go = function (){
		//这两种情况表示还在切换中
		if ( (speed > 0 && parseInt(list.style.left) < left) || (speed < 0 && parseInt(list.style.left) > left)) {
		list.style.left = parseInt(list.style.left) + speed + 'px';
		setTimeout(go, inteval); //继续执行切换go()函数
		}
		else {
		list.style.left = left + 'px';
		if(left>-600){
		list.style.left = -600 * len + 'px';
		}
		if(left<(-600 * len)) {
		list.style.left = '-600px';
		}
		animated = false; //切换完成
		}
		}
		go();
		}
		//用于为按钮添加样式
		function showButton() {
		//先找出原来有.on类的按钮，并移除其类
		for (var i = 0; i < buttons.length ; i++) {
		if( buttons[i].className == 'on'){
		buttons[i].className = '';
		break;
		}
		}
		//为当前按钮添加类
		buttons[index - 1].className = 'on';
		}
		//自动播放
		function play() {
			timer = setTimeout(function () {
			next.onclick();
			play();
			}, interval);
		}
		//清除定时器
		function stop() {
		clearTimeout(timer);
		}
		//右点击
		next.onclick = function () {
		if (animated) { //如果切换还在进行，则直接结束，直到切换完成
		return;
		}
		if (index == 5) {
		index = 1; 
		}
		else {
		index += 1;
		}
		animate(-600);
		showButton();
		}
		//左点击
		prev.onclick = function () {
		if (animated) { //如果切换还在进行，则直接结束，直到切换完成
		return;
		}
		if (index == 1) {
		index = 5;
		}
		else {
		index -= 1;
		}
		animate(600);
		showButton();
		}
		for (var i = 0; i < buttons.length; i++) {
		buttons[i].onclick = function () {
		if (animated) { //如果切换还在进行，则直接结束，直到切换完成
		return;
		}
		if(this.className == 'on') { //如果点击的按钮是当前的按钮，不切换，结束
		return;
		}
		//获取按钮的自定义属性index，用于得到索引值
		var myIndex = parseInt(this.getAttribute('index'));
		var offset = -600 * (myIndex - index); //计算总的位移量
		animate(offset);
		index = myIndex; //将新的索引值赋值index
		showButton();
		}
		}
		container.onmouseover = stop;//父容器的移入移出事件
		container.onmouseout = play;
		play(); //调用自动播放函数
		}
 </script>
</head>
<body>
 <div id="container">
<div id="list" style="left: -600px;">
<img src="image/1.jpg" alt="1"/>
<img src="image/2.jpg" alt="2"/>
<img src="image/3.jpg" alt="3"/>
<img src="image/4.jpg" alt="4"/>
<img src="image/6.jpg" alt="5"/>

</div>
<div id="buttons">
<span index="1" class="on"></span>
<span index="2"></span>
<span index="3"></span>
<span index="4"></span>
<span index="5"></span>
</div>
<a href=" " id="prev" class="arrow"><</a>
<a href="javascript:;" id="next" class="arrow">></a>
</div>
</body>
</html>
