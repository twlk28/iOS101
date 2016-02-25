# NSCoding



## 术语
* 文件写入，对象持久化，Archiver，编码；都是表达 `MEMO -> DISK` 的过程
* 文件读取，对象序列化，Unarchiver，解码；都是表达 `DISK -> MEMO` 的过程

## Demo
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
    
    required  init?(coder aDecoder: NSCoder) {
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

## ref
* [iOS沙盒目录结构解析](http://blog.csdn.net/wzzvictory/article/details/18269713)

