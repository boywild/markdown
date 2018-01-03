#js触摸事件
该类事件会在用户手指放在屏幕上面时，在屏幕上滑动时，或从屏幕上移开时触发。具体来说有以下几个触摸事件。
1. `touchstart`
当手指放在屏幕上触发。
2. `touchmove`
当手指在屏幕上滑动时，连续地触发。
3. `touchend`
当手指从屏幕上离开时触发。
4. `touchcancel`
当系统停止跟踪时触发，系统什么时候取消，文档没有明确的说明。

**【总】**以上四个，是w3c提供的触摸事件，只针对触摸设备，最常用的是前三个。
由于触摸会导致屏幕动来动去，所以可以会在这些事件的事件处理函数内使用event.preventDefault()，来阻止屏幕的默认滚动。

除了常用的DOM属性，触摸事件还包含下列三个用于跟踪触摸的属性。
1. `touches`
表示当前跟踪的触摸操作的touch对象的数组。
当一个手指在触屏上时，event.touches.length=1,
当两个手指在触屏上时，event.touches.length=2，以此类推。
2. `targetTouches`
特定于事件目标的touch对象数组。
因为touch事件是会冒泡的，所以利用这个属性指出目标对象。
3. `changedTouches`
表示自上次触摸以来发生了什么改变的touch对象的数组。
每个`touch`对象都包含下列几个属性：
* `clientX`：触摸目标在视口中的x坐标。
* `clientY`：触摸目标在视口中的y坐标。
* `identifier`：标识触摸的唯一ID。
* `pageX`：触摸目标在页面中的x坐标。
* `pageY`：触摸目标在页面中的y坐标。
* `screenX`：触摸目标在屏幕中的x坐标。
* `screenY`：触摸目标在屏幕中的y坐标。
* `target`：触摸的DOM节点目标。

【如何使用呢？】
```js
EventUtil.addHandler(div,"touchstart",function(event){
    div.innerHTML=event.touches[0].clientX+','+event.touches[0].clientY;
});
EventUtil.addHandler(div,"touchmove",function(event){
    event.preventDefault();
    div.innerHTML=event.touches[0].clientX;
});
EventUtil.addHandler(div,"touchend",function(event){
    div.innerHTML=event.changedTouches[0].clientY;
});
```

使用`clientX`时，必须要指明具体的`touch`对象，而不要直接指明数组。
* `event.touches[0]`
在`touchend`事件处理函数中，当该事件发生时，`touches`里面已经没有任何的`touch`对象了，此时，就要使用`changeTouches`集合。

**手势事件**
* `gesturestart`：当一个手指已经按在屏幕上，而另一个手指又触摸在屏幕时触发。
* `gesturechange`：当触摸屏幕的任何一个手指的位置发生变化时触发。
* `gestureend`：当任何一个手指从屏幕上面移开时触发。

【注意】只有两个手指都触摸到事件的接收容器时才触发这些手势事件
**触摸事件与手势事件之间的关系**
1. 当一个手指放在屏幕上时，会触发`touchstart`事件
2. 如果另一个手指又放在了屏幕上，则会触发`gesturestart`事件
3. 随后触发基于该手指的`touchstart`事件
4. 如果一个或两个手指在屏幕上滑动，将会触发`gesturechange`事件
5. 但只要有一个手指移开，则会触发`gestureend`事件，以后将不会再触发`gesturechange`
紧接着又会触发第二根手指的`touchend`
6. 提起第一根手指，触发`touchend`

**注意：**
触发`touchstart`！注意，多根手指在屏幕上，提起一根，会刷新一次全局`touch`！重新触发第一根手指的`touchstart`

**手势的专有属性**
`rotation`：表示手指变化引起的旋转角度，负值表示逆时针，正值表示顺时针，从零开始。
`scale`：表示两个手指之间的距离情况，向内收缩会缩短距离，这个值从1开始，并随距离拉大而增长。

```js
function touches(ev) {
    if (ev.touches.length == 1) {
        var oDiv = document.getElementById('div1');
        switch (ev.type) {
            case 'touchstart':
                oDiv.innerHTML = 'Touch start(' + ev.touches[0].clientX + ', ' + ev.touches[0].clientY + ')';
                ev.preventDefault(); //阻止出现滚动条 
                break;
            case 'touchend':
                oDiv.innerHTML = 'Touch end(' + ev.changedTouches[0].clientX + ', ' + ev.changedTouches[0].clientY + ')';
                break;
            case 'touchmove':
                oDiv.innerHTML = 'Touch move(' + ev.changedTouches[0].clientX + ', ' + ev.changedTouches[0].clientY + ')';
                break;

        }
    }
}
document.addEventListener('touchstart', touches, false);
document.addEventListener('touchend', touches, false);
document.addEventListener('touchmove', touches, false);

window.onload = function() {
    function gesture(ev) {
        var div = document.getElementById('div1');
        switch (ev.type) {
            case 'gesturestart':
                div.innerHTML = 'Gesture start (rotation=' + ev.rotation + ', scale=' + ev.scale + ')';
                ev.preventDefault();
                break;
            case 'gestureend':
                div.innerHTML = 'Gesture End (rotation=' + ev.rotation + ', scale=' + ev.scale + ')';
                break;
            case 'gesturechange':
                div.innerHTML = 'Gesture Change (rotation=' + ev.rotation + ', scale=' + ev.scale + ')';
                break;
        }
    }
    document.addEventListener('gesturestart', gesture, false);
    document.addEventListener('gestureend', gesture, false);
    document.addEventListener('gesturechange', gesture, false);
}
```

```js
//touchstart事件  
function touchSatrtFunc(e) {
    //evt.preventDefault(); //阻止触摸时浏览器的缩放、滚动条滚动等  
    var touch = e.touches[0]; //获取第一个触点  
    var x = Number(touch.pageX); //页面触点X坐标  
    var y = Number(touch.pageY); //页面触点Y坐标  
    //记录触点初始位置  
    startX = x;
    startY = y;
}
//touchmove事件 
function touchMoveFunc(e) {
    //evt.preventDefault(); //阻止触摸时浏览器的缩放、滚动条滚动等  
    var touch = evt.touches[0]; //获取第一个触点  
    var x = Number(touch.pageX); //页面触点X坐标  
    var y = Number(touch.pageY); //页面触点Y坐标  
    var text = 'TouchMove事件触发：（' + x + ', ' + y + '）';
    //判断滑动方向  
    if (x - startX != 0) {
        //左右滑动  
    }
    if (y - startY != 0) {
        //上下滑动  
    }
}
```
