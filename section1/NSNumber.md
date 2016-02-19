# NSNumber

* Swift 的数值有如下类型
	* 8 16 32 64 位的 有符/无符的 整型
	* 32 64 80 位的 浮点型
	* Int 根据平台映映射为 Int32 或 Int64


* OC 的数值类型只提供 NSNumber 类型
	* NSNumber构造：封装以上类型 以及 bool
	* NSNumber实例属性：提供以上类型 以及 bool 的取值
		* 值类型转换过程中，高位截取或补0
	* NSNumber实例方法：
		* `func compare(_ otherNumber: NSNumber) -> NSComparisonResult`
		* `func isEqualToNumber(_ number: NSNumber) -> Bool`
