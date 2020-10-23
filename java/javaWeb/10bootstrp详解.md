### 1.响应式布局bootstrap使用

```html
1.bootstrap基于jQuery，因此要导入jquery.js;
	导入bootstrap.css文件
    导入jquery.js
    导入bootstrap.js
	<!--导入css-->
	<link rel="stylesheet" href="css/bootstrap.min.css" />
	<!-- 导入jquery-->
	<script type="text/javascript" src="js/jquery-1.11.0.js" ></script>
	<!--导入bootstrap.js -->
	<script type="text/javascript" src="js/bootstrap.min.js" ></script>
2.导入支持移动设备，根据移动设备的屏幕大小，以1:1的比例呈现内容；
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
    当content的值为： maximum-scale=1.0 与 user-scalable=no 一起使用。可以禁用缩放功能，用户只能滚动屏幕，就能让您的网站看上去更像原生应用的感觉。
3.将所有内容放到布局容器中：
	.container类用于固定宽度，并支持响应式布局的容器。
	<div class="container">
        ...
	</div>
	.container-fluid类用于100%宽度，占据全部视口（Viewport）的容器；
	<div class="container-fluid">
        ...
	</div>
```

### 2.栅格系统

```html
大屏幕 大于1200       col-lg-2 
中屏幕 大于992<1200   col-md-3
小屏幕 大于768<922    col-sm-6
最小屏 小于768        col-xs-12
布局：
<div class="container">
    <div class="row">
        <div class="col-lg-3 col-md-3 col-sm-3" style="background: red;text-align: center;">1</div>
        <div class="col-lg-3 col-md-3 col-sm-3" style="background: red;text-align: center;margin-left: 10px;">2</div>
        <div class="col-lg-3 col-md-3 col-sm-3" style="background: red;text-align: center;margin-left: 10px;">3</div>
        <div class="col-lg-3 col-md-3 col-sm-3" style="background: red;text-align: center;margin-left: 10px;">4</div>
    </div>
</div>
说明：将所有内容放到.container容器中，并且把栅格放入.row行容器中，这些栅格自动占据一行；
```

