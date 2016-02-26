# NSCoding

## 术语
* 文件写入，对象持久化，Archiver，编码；都是表达 `MEMO -> DISK` 的过程
* 文件读取，对象序列化，Unarchiver，解码；都是表达 `DISK -> MEMO` 的过程

### 相关
* 对特定文件类型的读取或保存
    * NSKeyedArchiver NSKeyedUnarchiver 对`plist <-> AnyObject`的转换
    * NSJSONSerialization 对 `JSON <-> AnyObject` 的转换
* 本篇未涉及的iOS持久化技术
    * 属性列表 NSUserDefaults
    * 数据库 SQLite CoreData    

## NSKeyedArchiver
```swift
	// #对象持久化：对象编码成二进制数据，然后写出文件
    // ... 1 构造 可变的NSData
    // ... 2 通过 可变的NSData 构造 NSKeyedArchiver
    // ... 3 NSKeyedArchiver 对AnyObject编码
    // ... ... 编码解码链涉及的对象需实现 NSCoding
    // ... 4 NSKeyedArchiver 结束编码
    // ... 5 可变的NSData writeToFile
    func writeToDisk() {
        let list:[VO] = [VO(), VO()]
        guard let savedPath = getVOFilePath() else {
            return
        }
        
        let data = NSMutableData()
        let archiver = NSKeyedArchiver(forWritingWithMutableData: data)
        archiver.encodeObject(list, forKey: "VOList")
        archiver.finishEncoding()
        data.writeToFile(savedPath, atomically: true)
    }
    
    // #对象序列化：文件二进制数据 解码成 对象
    // ... 1 通过 filePath 构造 NSData
    // ... 2 通过 NSData 构造 NSKeyedUnarchiver
    // ... 3 NSKeyedUnarchiver 传入 KEY 来解码，转型
    // ... ... 编码解码链涉及的对象需实现 NSCoding
    // ... 4 NSKeyedUnarchiver 结束解码
    func loadFromDisk() -> [VO]? {
        guard let savedPath = getVOFilePath() else {
            return nil
        }
        if NSFileManager.defaultManager().fileExistsAtPath(savedPath) {
            if let data = NSData(contentsOfFile: savedPath) {
                let unarchiver = NSKeyedUnarchiver(forReadingWithData: data)
                let lists = unarchiver.decodeObjectForKey("VOList") as? [VO]
                unarchiver.finishDecoding()
                return lists
            }
        }
        return nil
    }

    func getVOFilePath() -> String?{
        let paths = NSSearchPathForDirectoriesInDomains(.DocumentDirectory, .UserDomainMask, true)
        guard paths.count > 0 else {
            return nil
        }
        return NSString(string: paths[0]).stringByAppendingPathComponent("demo.plist")
    }

```

```swift
class VO: NSObject, NSCoding {
    var name:String
    var age:Int
    
    override init(){
        name = "name"
        age = 18
        super.init()
    }
    
    required init?(coder aDecoder: NSCoder) {
        if let name = aDecoder.decodeObjectForKey("Name") as? String {
            self.name = name
        } else {
            self.name = "name"
        }
        if let age = aDecoder.decodeObjectForKey("Age") as? Int {
            self.age = age
        } else {
            self.age = 18
        }
        super.init()
    }
    
    func encodeWithCoder(aCoder: NSCoder) {
        aCoder.encodeObject(name, forKey: "Name")
        aCoder.encodeObject(age, forKey: "Age")
    }
    
}
```

## NSJSONSerialization
```swift
    func getJSONFilePath() -> String?{
        let paths = NSSearchPathForDirectoriesInDomains(.DocumentDirectory, .UserDomainMask, true)
        guard paths.count > 0 else {
            return nil
        }
        return NSString(string: paths[0]).stringByAppendingPathComponent("demo.json")
    }
    func loadFile() -> AnyObject? {
        guard let savedPath = getJSONFilePath() else {
            return nil
        }
        
        do {
            if let data = NSData(contentsOfFile: savedPath) {
                let obj = try NSJSONSerialization.JSONObjectWithData(data, options: .MutableContainers)
                return obj
            }
        } catch {
            return nil
        }
    
        return nil
    }
    
    func saveFile() {
       let savedData: [String: AnyObject] = ["name": "adi", "age": 180]
        
        guard NSJSONSerialization.isValidJSONObject(savedData) else {
            return
        }
        guard let savedPath = getJSONFilePath() else {
            return
        }
        do {
            let data = try NSJSONSerialization.dataWithJSONObject(savedData, options: .PrettyPrinted)
            data.writeToFile(savedPath, atomically: true)
        } catch {
            return
        }
    }

```

### NSJSONReadingOptions
``` swift
// mutableContainers: 解析出来的 JSONObject 的数组和字典是可变的
func mutableContainers() {
    let jsonStr:NSString =  NSString(string: " [1, 2, 3] ")
    let jsonData:NSData = jsonStr.dataUsingEncoding(NSUTF8StringEncoding)!
    do {
        if let jsonObj:AnyObject = try NSJSONSerialization.JSONObjectWithData(jsonData, options: .MutableContainers) {
            
            if let jsonObj = jsonObj as? NSMutableArray {
                jsonObj.addObject(4)
            }
        }
    } catch {
        
    }
}


// allowFragment: JSON有三种表达 对象 数组 值，这里指允许值的表达
func allowFragment() {
    let jsonStr:NSString =  NSString(string: " \" A JSON VALUE \" ")
    let jsonData:NSData = jsonStr.dataUsingEncoding(NSUTF8StringEncoding)!
    do {
        if let jsonObj:AnyObject = try NSJSONSerialization.JSONObjectWithData(jsonData, options: .AllowFragments) {
            jsonObj
        }
        
    } catch {
        
    }
}

// mutableLeaves: 无效，允许字符串是可变动
func mutableLeaves() {
//    let jsonStr:NSString =  NSString(string: " { \"attr\": \"val\"} ")
    let jsonStr:NSString =  NSString(string: " \" A JSON VALUE \" ")
    let jsonData:NSData = jsonStr.dataUsingEncoding(NSUTF8StringEncoding)!
    do {
        if let jsonObj:AnyObject = try NSJSONSerialization.JSONObjectWithData(jsonData, options: [.MutableLeaves, .AllowFragments]) {
            
            if let jsonObj = jsonObj as? NSMutableString {
                jsonObj.appendString(" ADDON STRING ")
            }
        }
        
    } catch {
        
    }
}    
```

## 疑问
1. Class:NSObject:NSCoding <-> JSONObject 
* 为啥需要这类转换？
    * `json2obj`: web用户操作 -> http json -> json parse, for init XClass, maybe VO Bean -> DB
    * `obj2json`: DB -> Bean -> JSONStringify -> http json -> webUI Update
* 如何实现：http://stackoverflow.com/questions/31971256/how-to-convert-a-swift-object-to-a-dictionary

```swift

extension VO {
    // dict -> VO
    convenience init(dict:[String: AnyObject]) {
        var nameValue:String
        var ageValue:Int
        
        if let name = dict["name"] as? String {
            nameValue = name
        } else {
            nameValue = "adi"
        }
        
        if let age = dict["age"] as? Int {
            ageValue = age
        } else {
            ageValue = 18
        }
        
        self.init(nameValue, ageValue)
    }

    // VO -> dict
    func toDict() -> [String: AnyObject] {
        var dict = [String: AnyObject]()
        let otherSelf = Mirror(reflecting: self)
        for item in otherSelf.children {
            if let key = item.label {
                dict[key] = item.value as? AnyObject
            }
        }
        return dict
    }
}
```

## ref
* [iOS沙盒目录结构解析](http://blog.csdn.net/wzzvictory/article/details/18269713)
* [swift-json](http://swiftcafe.io/2015/07/18/swift-json/)
