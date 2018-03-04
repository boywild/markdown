#AES加解密流程及方法

##流程图
###加密
AES方法是支持AES-128、AES-192和AES-256的，加密过程中使用哪种加密方式取决于传入key的类型，否则就会按照AES-256的方式加密
![](https://ws1.sinaimg.cn/large/aaf377eegy1foxh8umwlzj20ta0if0vi.jpg)
###解密
由于加密后的密文为128位的字符串，那么解密时，需要将其转为Base64编码的格式。
那么就需要先使用方法CryptoJS.enc.Hex.parse转为十六进制，再使用CryptoJS.enc.Base64.stringify将其变为Base64编码的字符串，此时才可以传入CryptoJS.AES.decrypt方法中对其进行解密。
![](https://ws1.sinaimg.cn/large/aaf377eegy1foxh8vboryj20ta0g940p.jpg)
##流程
首先准备一份明文和秘钥：

```js
var plaintText = 'aaaaaaaaaaaaaaaa'; // 明文
var keyStr = 'bbbbbbbbbbbbbbbb'; // 一般key为一个字符串  
```
    AES方法是支持AES-128、AES-192和AES-256的，加密过程中使用哪种加密方式取决于传入key的类型，否则就会按照AES-256的方式加密。
	由于Java就是按照128bit给的，但是由于是一个字符串，需要先在前端将其转为128bit的才行。
最开始以为使用CryptoJS.enc.Hex.parse就可以正确地将其转为128bit的key。但是不然... 
经过多次尝试，需要使用CryptoJS.enc.Utf8.parse方法才可以将key转为128bit的。好吧，既然说了是多次尝试，那么就不知道原因了，后期再对其进行更深入的研究。

一、字符串类型的key用之前需要用uft8先parse一下才能用

```js
var key = CryptoJS.enc.Utf8.parse(keyStr);  
```
　　由于后端使用的是PKCS5Padding，但是在使用CryptoJS的时候发现根本没有这个偏移，查询后发现PKCS5Padding和PKCS7Padding是一样的东东，使用时默认就是按照PKCS7Padding进行偏移的。
二、加密

```js
var encryptedData = CryptoJS.AES.encrypt(plaintText, key, {  
    mode: CryptoJS.mode.ECB,
    padding: CryptoJS.pad.Pkcs7
});
```
　　由于CryptoJS生成的密文是一个对象，如果直接将其转为字符串是一个Base64编码过的，在encryptedData.ciphertext上的属性转为字符串才是后端需要的格式。
三、转换成base64字符串

```js
var encryptedBase64Str = encryptedData.toString();
console.log(encryptedBase64Str);//'RJcecVhTqCHHnlibzTypzuDvG8kjWC+ot8JuxWVdLgY='
```
四、需要读取encryptedData上的ciphertext.toString()才能拿到跟Java一样的密文

```js
var encryptedStr = encryptedData.ciphertext.toString();  
console.log(encryptedStr); //'44971e715853a821c79e589bcd3ca9cee0ef1bc923582fa8b7c26ec5655d2e06' 
```
　　由于加密后的密文为128位的字符串，那么解密时，需要将其转为Base64编码的格式。
那么就需要先使用方法CryptoJS.enc.Hex.parse转为十六进制，再使用CryptoJS.enc.Base64.stringify将其变为Base64编码的字符串，此时才可以传入CryptoJS.AES.decrypt方法中对其进行解密。

五、拿到字符串类型的密文需要先将其用Hex方法parse一下

```js
var encryptedHexStr = CryptoJS.enc.Hex.parse(encryptedStr);
```
六、将密文转为Base64的字符串
只有Base64类型的字符串密文才能对其进行解密

```js
var encryptedBase64Str = CryptoJS.enc.Base64.stringify(encryptedHexStr);  
```

七、使用转为Base64编码后的字符串即可传入CryptoJS.AES.decrypt方法中进行解密操作

```js
var decryptedData = CryptoJS.AES.decrypt(encryptedBase64Str, key, {  
    mode: CryptoJS.mode.ECB,
    padding: CryptoJS.pad.Pkcs7
});
```
经过CryptoJS解密后，依然是一个对象，将其变成明文就需要按照Utf8格式转为字符串。

八、解密后，需要按照Utf8的方式将明文转位字符串

```js
var decryptedStr = decryptedData.toString(CryptoJS.enc.Utf8);  
console.log(decryptedStr); // 'aaaaaaaaaaaaaaaa'
```
##方法

```js
var plaintText = 'aaaaaaaaaaaaaaaa'; //明文
var keyStr = 'bbbbbbbbbbbbbbbb'; //一般key为一个字符串 
var key = CryptoJS.enc.Utf8.parse(keyStr);
var encryptedData = CryptoJS.AES.encrypt(plaintText, key, {  
    mode: CryptoJS.mode.ECB,
    padding: CryptoJS.pad.Pkcs7
});
var encryptedStr = encryptedData.ciphertext.toString(); 
var encryptedHexStr = CryptoJS.enc.Hex.parse(encryptedStr);
var encryptedBase64Str = CryptoJS.enc.Base64.stringify(encryptedHexStr);  
var decryptedData = CryptoJS.AES.decrypt(encryptedBase64Str, key, {  
    mode: CryptoJS.mode.ECB,
    padding: CryptoJS.pad.Pkcs7
});
var decryptedStr = decryptedData.toString(CryptoJS.enc.Utf8);  
```