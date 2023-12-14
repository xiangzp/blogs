# 中文的顿号 、不能base64，报错‘ The string to be encoded contains characters outside of the Latin1 range’ 为什么

中文的顿号 "、" 在Unicode编码中属于CJK标点符号范畴，而Latin1编码只包含了西欧语言使用的字符集。因此，当尝试使用Latin1编码来对含有中文顿号的字符串进行Base64编码时，会出现报错“ The string to be encoded contains characters outside of the Latin1 range”，这是因为Latin1编码无法正确处理非西欧语言的字符。要解决这个问题，可以使用其他支持Unicode字符编码的方法来进行Base64编码。

```js
// 将包含中文顿号的字符串进行Base64编码
function base64EncodeUnicode(str) {
  // 先将 Unicode 字符串转换为 UTF-8 编码格式
  const utf8str = encodeURIComponent(str).replace(/%([0-9A-F]{2})/g, function(match, p1) {
    return String.fromCharCode('0x' + p1);
  });
  // 使用 btoa 函数对 UTF-8 字符串进行 Base64 编码
  return btoa(utf8str);
}

// 将经过 Base64 编码的字符串解码为原始字符串
function base64DecodeUnicode(str) {
  // 使用 atob 函数对 Base64 编码的字符串进行解码
  const utf8str = atob(str);
  // 将 UTF-8 字符串转换回 Unicode 格式
  return decodeURIComponent(utf8str.split('').map(function(c) {
    return '%' + ('00' + c.charCodeAt(0).toString(16)).slice(-2);
  }).join(''));
}

// 示例
const originalString = "这是一个包含中文顿号、的字符串";
const encodedString = base64EncodeUnicode(originalString);
console.log("Base64 编码后的字符串:", encodedString);

const decodedString = base64DecodeUnicode(encodedString);
console.log("解码后的字符串:", decodedString);
```

通过上述 JavaScript 代码，你可以成功地对包含中文顿号的字符串进行Base64编码和解码，并且避免了使用已经被标记为过时的`unescape`函数。