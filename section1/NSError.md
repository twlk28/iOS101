# NSError



## 目录
1. Exception vs Error
2. Error Demo



## 正文

### [Exception vs Error][4]

1. Exception：
	* 概念：programming errors；编译期语法正确，运行时求值出现错误
	* 例子：
		* 数组越界访问，得到 NSRangeException
		* 向一个无法响应某个消息的 NSObject对象发送了这个消息，得到 NSInvalidArgumentException异常和"unrecognized selector sent to instance"的信息；

2. Error：
	* 概念：应用级别的错误状态；应用级别指公认的HTTP、POSIX等等 或自身业务系统制定的
	* 例子：
		* 登录用户时用户名密码不匹配
		* 或者试图从某个文件中读取数据生成 NSData对象时发生问题
			* 比如文件被意外修改
		
3. Error构造
	* domain: Error所属的应用领域，[详见][2]，也可理解为命名空间。
	* code: Error编码，应用领域下的细分。
	* userInfo: 错误的详细说明；字典类型，内置Key如下，也可自定义Key
		* NSLocalizedDescriptionKey: 【描述短语】
		* NSLocalizedFailureReasonErrorKey:【产生原因】
		* NSLocalizedRecoveryOptionsErrorKey:【消除错误的几种方法】
		* NSLocalizedRecoverySuggestionErrorKey:【消除错误建议的方法】
		* NSRecoveryAttempterErrorKey:【消除错误的每种方法对应的详细步骤描述】

### Error Demo
```swift
// #a 声明函数，抛出错误
// ... 1声明
func execThrow() throws -> Void{
    let err = NSError(domain: "HTTP", code: 404, userInfo: nil)
    throw err
}
// ... 2调用
do {
    try execThrow()
} catch let err as NSError {
    print(err.description)
}

// #b 声明函数，返回错误
// ... 1声明
func execReturnError(isError:Bool) -> NSError? {
    if isError {
        let err = NSError(domain: "HTTP", code: 404, userInfo: nil)
        return err
    }
    return nil
}
// ... 2调用
let err = execReturnError(true)
if let err = err {
    print(err)
}

// #c b的扩展，返回处理结果，处理结果包含错误
// ... 1声明
enum ResultOfExec<T> {
    case Success(T)
    case Error(NSError)
}
func execReturnError2(isError:Bool) -> ResultOfExec<String> {
    if isError {
        let err = NSError(domain: "HTTP", code: 404, userInfo: nil)
        return ResultOfExec.Error(err)
    }
    return ResultOfExec.Success("jsonString")
}
// ... 2调用
let result = execReturnError2(false)
switch result {
case let .Success(data):
    print(data)
case let .Error(err):
    print(err)
}
```

	

## 扩展阅读
* [NSError 最佳实践][1]
* [NSError 特定应用领域的错误编码列表][2]

--
[1]: http://swifter.tips/error-handle/
[2]: http://nshipster.com/nserror/
[3]: https://en.wikipedia.org/wiki/Uniform_Resource_Identifier#Syntax
[4]: https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Exceptions/Exceptions.html