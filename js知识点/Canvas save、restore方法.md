###Canvas save(),restore()方法
* `save()`和`restore()`方法是绘制复杂图形必不可少的方法.它们分别是用来保存和恢复 `canvas` 状态的，都没有参数。
* 每一次调用 `save` 方法，当前的状态就会被推入堆中保存起来。这种状态包括：
当前应用的变形（即移动，旋转和缩放）strokeStyle, fillStyle, globalAlpha, lineWidth, lineCap, lineJoin, miterLimit, shadowOffsetX, shadowOffsetY, shadowBlur, shadowColor, globalCompositeOperation 的值
* 每一次调用 `restore` 方法，上一个保存的状态就从堆中弹出，所有设定都恢复。

![这里写图片描述](http://img.blog.csdn.net/20161007233840264)
```js

	window.onload=function(){
	    var ctx=document.getElementById("myCanvas").getContext("2d");
	    ctx.fillStyle='#064af5';
	    ctx.fillRect(0,0,600,600);

	    ctx.save();//将下述区域保存在堆栈中
	    ctx.fillStyle='#f00';
	    ctx.fillRect(50,50,500,500);
	    ctx.restore();//将最近一次保存在堆栈中的图形释放掉，恢复到最近一次保存的上次图形

	    ctx.save();//将下述区域保存在堆栈中
	    ctx.fillStyle='pink';
	    ctx.fillRect(100,100,400,400);
	    ctx.restore();//将最近一次保存在堆栈中的图形释放掉，恢复到最近一次保存的上次图形

	    ctx.fillRect(150,150,300,300);//将最近一次保存在堆栈中的图形释放掉，恢复到最近一次保存的上次图形


	}
```

每调用一次`restore`方法，画布状态就会返回到当前`save`状态的上一个状态
![这里写图片描述](http://img.blog.csdn.net/20161007233915831)
