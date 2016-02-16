# NSData

## 构造
* initWithData: @Q为啥通过一个NSData实例来构造 
* `<base64>`
* `<Bytes>`
* `<FilePath>`
* `<URL>`

## 访问数据
* bytes 属性
* description 属性
* -enumerateByteRangesUsingBlock: @Q
* -getBytes:length: 转成Buff
* -getBytes:range: 转成Buff
* -subdataWithRange: 按range截取后返回NSData
* -rangeOfData:options:range: 查找后返回range
* [Working With Binary Data][1]

## 转成Base64编码

## Testing Data
* equal 运算
* length 属性

## 数据存储
* 写到文件
* 写到URL


--
[1]: https://developer.apple.com/library/tvos/documentation/Cocoa/Conceptual/BinaryData/Tasks/WorkingBinaryData.html