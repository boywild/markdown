#SVG之text


##<text>基本属性
`x,y,stroke,fill,font,styles`

(x,y)用于指定文字起始位置。准确的说，x指定文字最左侧坐标位置，y指定文字baseline所处y轴位置。
fill的默认为black，stroke默认为none。如果设置了stroke，字的边缘会再“描一层边”。如果设置了stroke并将fill设为none，呈现为空心字。
css中影响字体样式的属性同样可以作用在<text>上：`font-size, font-weight, font-family, font-style, font-decoration, word-spacing, letter-spacing`。

```html
<g id="coordinates" stroke="black" stroke-width="1">
    <path d="M10 0v90m0 -60h200m-200 30h200m-200 30h200"></path>
</g>
<g id="text">
    <text x="10" y="30">First Line</text>
    <text x="10" y="60" stroke="black">Second Line</text>
    <text x="10" y="90" stroke="blue" fill="none" stroke-width=".5">Third Line</text>
</g>
```
![](https://ws1.sinaimg.cn/large/aaf377eegy1fo6wzpx140j209004c0su.jpg)
##对齐
默认<text>从起始位置(x,y)开始展示。但由于在svg中无法事先知道<text>的长度，所以无法仅通过改变(x,y)让<text>的中轴或结束位置位于指定位置。
但svg提供了一种更简单直接的方法实现这些对齐方式。

* `text-anchor`属性可以改变(x,y)作为起始坐标的定义。
* `text-anchor="start"`时，(x,y)为<text>的起始坐标。
* `text-anchor="middle"`时，(x,y)为<text>的中轴坐标。
* `text-anchor="end"`时，(x,y)为<text>的结束坐标。

```js
<g style="font-size: 14pt;">
    <path d="M 100 10 100 100" style="stroke: gray; fill: none;"/>
    <text x="100" y="30" style="text-anchor: start">Start</text>
    <text x="100" y="60" style="text-anchor: middle">Middle</text>
    <text x="100" y="90" style="text-anchor: end">End</text>
</g>
```
![](https://ws1.sinaimg.cn/large/aaf377eegy1fo6wzptelcj206r045q2u.jpg)

##<tspan>
如果要实现在一整段文字中，让部分文字呈现出不同的样式，只用<text>是不现实的，在不知道一个<text>在哪儿结束的情况下，无法让另一个<text>紧接在其后面。
<tspan>允许被嵌入在<text>内部来实现以上场景。
<tspan>除了<text>拥有的属性外，还有另外两个属性

* dx
* dy

(dx,dy)可以将<tspan>内的文字，以其初始位置为起点(0,0)，偏移(dx,dy)
(x,y)是将<tspan>内的文字定位到其坐标系的(x,y)位置。

```js
<g id="coordinates" fill="none" stroke="black" stroke-width="1">
    <path d="M10 0v30h200m-190 0v30h190m-180 0v30h180"></path>
</g>
<g id="text" font-size="2rem">
    <text x="10" y="30">first line
        <tspan x="20" y="60">second line</tspan>
        <tspan x="30" dy="30">third line</tspan>
    </text>
</g>
```
![](https://ws1.sinaimg.cn/large/aaf377eegy1fo6wzq3pvpj207j03lmx3.jpg)

dx,dy还可以这么玩

```js
<text x="30" y="30" font-size="2rem">
    It’s
    <tspan dx="0 4 -3 5 -4 6" dy="0 -3 7 3 -2 -8"
           rotate="5 10 -5 -20 0 15">shaken</tspan>,
    not stirred.
</text>
```
![](https://ws1.sinaimg.cn/large/aaf377eegy1fo6wzpsg0zj20c801ca9x.jpg)

"shaken"一共六个字符，dx,dy,rotate分别各有六个数值，空格或逗号隔开，每个数值对相作用于应次序的字母。rotate可以对字符做旋转。
如果dx（或者dy,rotate）参数个数小于<tspan>内的字符个数，最后一个dx(dy,rotate)参数值将作用于多出的字符。

另外，上标下标除了可以用dy来抬高或降低字符位置，还有一个baseline-shift样式可以直接定义上标下标

```js
<text x="20" y="25" style="font-size: 1.5rem;">
    C<tspan style="baseline-shift: sub;font-size: 1rem;">12</tspan>
    H<tspan style="baseline-shift: sub;font-size: 1rem">22</tspan>
    O<tspan style="baseline-shift: sub;font-size: 1rem">11</tspan> (sugar)
</text>
<text x="20" y="70" style="font-size: 1.5rem;">
    6.02 x 10<tspan baseline-shift="super" style="font-size:1rem">23</tspan>
    (Avogadro's number)
</text>
```
![](https://ws1.sinaimg.cn/large/aaf377eegy1fo6wzq0q4uj20b602bjr9.jpg)
##设置text长度及字符间隔
默认情况下无从获得<text>的长度，但可以通过textlength属性设置<text>长度。<text>内部字符会根据textLength自适应变化。通过lengthAdjust属性来设置字符变化规则。
lengthAdjust有两个可选属性值。

* spacing
* spacingAndGlyphs

spacing只调整字符之间的间隔；spacingAndGlyphs则会根据一定比例同时调整字符之间的间隔，以及字符本身宽度。

```js
<g style="font-size: 1.3rem;">
    <path d="M 20 10 20 70 M 220 10 220 70" style="stroke: gray;"></path>
    <text x="20" y="30" textLength="200" lengthAdjust="spacing">Two words</text>
    <text x="20" y="60" textLength="200" lengthAdjust="spacingAndGlyphs">Two words</text>
    <text x="20" y="90">Two words
        <tspan style="font-size: 10pt;">(normal length)</tspan>
    </text>
    <path d="M 20 100 20 170 M 90 100 90 170" style="stroke: gray;"></path>
    <text x="20" y="120" textLength="70" lengthAdjust="spacing">Two words</text>
    <text x="20" y="160" textLength="70" lengthAdjust="spacingAndGlyphs">Two words</text>
</g>
```
![](https://ws1.sinaimg.cn/large/aaf377eegy1fo6wzpvxvpj2080069dfv.jpg)
##垂直text
有两种方法实现垂直显示text。一种使用transform，一种是text特有的writing-mode：tb(top to bottom)样式。
书上用writing-mode + glyph-orientation-vertical的实现方式在chrome上没生效。
个人认为使用transform（或者writing-mode:tb）+ rotate + letter-spacing实现起来更为灵活。

```js
<text x="50" y="20" style="writing-mode: tb;letter-spacing:5px" rotate="-90" >Writing Vertical Text</text>
<text x="70" y="20" transform="rotate(90, 70, 20)" style="letter-spacing:5px" rotate="-90" >Writing Vertical Text</text>
```
![](https://ws1.sinaimg.cn/large/aaf377eegy1fo6wzppug2j201u08fwe9.jpg)
##textPath
内嵌于<text>中的<textpath>元素，通过xlink:href属性指向一个<path>元素，可以将其内部字符的baseline设置成指定的path。

```js
<defs>
    <path id="circle" d="M70 20a40 40 0 1 1 -1 0"></path>
</defs>
<text>
    <textPath xlink:href="#circle">
        Text following a circle.............
    </textPath>
</text>
```
![](http://ww1.sinaimg.cn/large/aaf377eegy1fo6wzpssz4j206105eq2v.jpg)

超出<path>长度部分的文字将被隐藏。
默认的，<textPath>的起始位置为<path>的起始位置。
<textPath>元素还有一个startOffset属性，可以调整<text>起始位置。

* startOffset为百分数时，<textPath>起始位置 = startOffset * <path>总长度。
* startOffset为具体数字时，<textPath>起始位置 = startOffset + <path>的起始位置。
利用startOffset和text-anchor，可以实现文字居中摆放。

```js
<defs>
    <path id="semi" d="M110 100a50 50 0 1 1 100 0"></path>
</defs>
<use xlink:href="#semi" stroke="grey" fill="none"></use>
<text text-anchor="middle">
    <textPath xlink:href="#semi" startOffset="50%">
        Middle
    </textPath>
</text>
```

##关于空白符
关键就说一点，svg没有换行符！svg默认会把所有单个或连续多个空格、tabs、换行符转成单个空格。即使在css中将white-space设置为pre，换行符依然会被转换成空格！