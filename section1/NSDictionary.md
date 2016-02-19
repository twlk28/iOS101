# NSDictionary

```swift

func exec() {
//    initializing()
//    accessingMember()
//    iterator()
//    sorting()
    filtering()
}
exec()

// prepare


// 构造 (初始化initializing 其他对象转换creating
func initializing() {
    let key1 = NSString(string: "1 2 3").componentsSeparatedByString(" ")
    let val1 = NSString(string: "a b c").componentsSeparatedByString(" ")

    let dict1 = NSDictionary(dictionary: [key1 : val1], copyItems: false)
    let dict2 = NSDictionary(objects: val1, forKeys: key1)

    dict1.count
    dict2.count
}

// 键名、键值 访问
func accessingMember() {
    let dict1 = NSDictionary(dictionary: ["a" : 1, "b" : 2, "c" : 1])
    
    dict1.allKeys
    dict1.allKeysForObject(1)
    
    dict1.allValues
    dict1.objectForKey("a")
}

// 迭代
func iterator() {
    let dict1 = NSDictionary(dictionary: ["a" : 1, "b" : 2, "c" : 1])

    var result: Int = 0
    dict1.enumerateKeysAndObjectsWithOptions(.Reverse) { (key, val, stop) -> Void in
        if let val = val as? Int where val == 2{
            stop.memory = true // stop failed?
            result = result + val
            [key, val]
        }
    }
    result
}


// 排序
func sorting() {
    let dict1 = NSDictionary(dictionary: ["a" : 1, "b" : 2, "c" : 1])
    
    // 按值排序 keys
    dict1.keysSortedByValueWithOptions(.Concurrent) { (prevVal, nextVal) -> NSComparisonResult in
        if let prevVal = prevVal as? Int, nextVal = nextVal as? Int {
            return prevVal < nextVal ? .OrderedAscending: .OrderedDescending
        }
        return .OrderedAscending
    }
}


// 过滤
func filtering() {
    let dict1 = NSDictionary(dictionary: ["a" : 1, "b" : 2, "c" : 1])
    
    dict1.keysOfEntriesWithOptions(.Reverse) { (itemKey, itemVal, stop) -> Bool in
        if let itemKey = itemKey as? String, itemVal = itemVal as? Int {
            return itemVal < 2 ? true : false
        }
        return false
    }
    
}

// 存储 写文件 @TODO
func storing() {
    
}

// 访问文件属性: 大小 类型 权限 所有者 修改时间 ... @TODO
func accessingFileAttr() {
    
}

```