#Math.random()来生成指定范围内（如：1-100）的随机数

##一、包括下线数字（lower）不包括上限数字（upper）
```js
/**
 * 产生随机整数，包含下限值，但不包括上限值
 * @param {Number} lower 下限
 * @param {Number} upper 上限
 * @return {Number} 返回在下限到上限之间的一个随机整数
 */
function random(lower, upper) {
	return Math.floor(Math.random() * (upper - lower)) + lower;
}
```
##二、包括下线数字（lower）也包括上限数字（upper）
```js

/**
 * 产生随机整数，包含下限值，包括上限值
 * @param {Number} lower 下限
 * @param {Number} upper 上限
 * @return {Number} 返回在下限到上限之间的一个随机整数
 */
function random(lower, upper) {
	return Math.floor(Math.random() * (upper - lower + 1)) + lower;
}
```

##三、扩展
```js

/**
 * 产生一个随机的rgb颜色
 * @return {String}  返回颜色rgb值字符串内容，如：rgb(201, 57, 96)
 */
function randomColor() {
	// 随机生成 rgb 值，每个颜色值在 0 - 255 之间
	var r = random(0, 256),
		g = random(0, 256),
		b = random(0, 256);
	// 连接字符串的结果
	var result = "rgb("+ r +","+ g +","+ b +")";
	// 返回结果
	return result;
}
```