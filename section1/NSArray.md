# NSArray

```swift
func exec(){
//    initialize()
//    query()
//    iterator()
//    indexes()
//    comparingArray()
//    derivingNewArrays()
    sorting()
    
}
exec()


// prepare
class Person: NSObject, NSCopying {
    var name: String
    var age = 100
    
    init(_ name: String) {
        self.name = name
        super.init()
    }
    init(_ name: String, _ age: Int){
        self.name = name
        self.age = age
        super.init()
    }
    
    func copyWithZone(zone: NSZone) -> AnyObject {
        return Person(self.name)
    }
}
func printName(withFirstItemOfArray array: NSArray) -> String? {
    if let item = array.firstObject as? Person {
        return item.name
    }
    return nil
}

// 构造
func initialize(){
    NSArray(array: [1, 2, 3])
    NSArray(array: [Person("a")], copyItems: true)
//    NSArray(contentsOfFile: "path")
    NSArray(objects: Person("a"), Person("b"))
}

// 查询
func query(){
    let item = Person("x")
    let a1 = NSArray(objects: Person("a"), Person("b"), item)

    a1.count
    a1.firstObject
    a1.lastObject
    
    // AnyObject -> Bool
    a1.containsObject(item)
    
    // index -> AnyObject
    a1.objectAtIndex(0)
    
    // index -> [AnyObject]
    a1.objectsAtIndexes(NSIndexSet(indexesInRange: NSRange(0...1)))

    // a1.getObjects(objects: AutoreleasingUnsafeMutablePointer<AnyObject?>, range: NSRange)
    
}

// 迭代/遍历/枚举
func iterator(){
    let a1 = NSArray(objects: Person("a"), Person("b"), Person("c"))
    let indexes = NSIndexSet(indexesInRange: NSRange(0...1))
    
    
    // # objectAtIndex
    for(var index = 0; index < a1.count; index++){
        a1.objectAtIndex(index)
    }
    
    // # NSEnumerator
    let iterator = a1.objectEnumerator()
    while let item:Person = iterator.nextObject() as? Person {
        item.name
    }
    
    // # NSFastEnumerator
    for item in a1 {
        if let item = item as? Person {
            item.name
        }
    }
    
    // # BlockEnumeration 可指定并发、枚举范围
    a1.enumerateObjectsAtIndexes(indexes, options: NSEnumerationOptions.Concurrent) { (item, index, stop) -> Void in
        if let item = item as? Person {
            item.name
        }
    }
}

//

// 获取索引或索引集
func indexes(){
    let item = Person("c")
    let a1 = NSArray(objects: Person("a"), Person("b"), item, item)
    let range = NSRange(0...1)
    let indexes = NSIndexSet(indexesInRange: range)
    
    // AnyObject -> index, 可指定 range
    a1.indexOfObject(item)
    a1.indexOfObject(item, inRange: range)
    a1.indexOfObjectIdenticalTo(item) // 返回最小索引
    
    // passingTest -> index, 可指定 NSIndexSet、NSEnumerationOptions
    a1.indexOfObjectAtIndexes(indexes, options: NSEnumerationOptions.Concurrent) { (item, index, stop) -> Bool in
        return true
    }
    
    // passingTest -> indexes, 可指定 NSIndexSet、NSEnumerationOptions
    a1.indexesOfObjectsAtIndexes(indexes, options: NSEnumerationOptions.Concurrent) { (item, index, stop) -> Bool in
        return true
    }
    
    // usingComparator -> indexes @TODO
    a1.indexOfObject(item, inSortedRange: NSRange(0...3), options: NSBinarySearchingOptions.InsertionIndex) { (prev, next) -> NSComparisonResult in
        if let prev = prev as? Person {
            if prev.name == "a" {
                prev.age
            }
        }
        
        return NSComparisonResult.OrderedSame
    }
    
}


// 与 Array 比较
func comparingArray(){
    let item = Person("c")
    let arr = Array(arrayLiteral: item, Person("a"))
    let a1 = NSArray(array: arr, copyItems: false)
    
    a1.firstObjectCommonWithArray([Person("a"), item])
    a1.isEqualToArray(arr)
}

// 生成新的数组
func derivingNewArrays(){
    let item = Person("c")
    let arr = Array(arrayLiteral: item, Person("a"))
    let a1 = NSArray(array: arr, copyItems: true)
    
    // 相当于 浅拷贝出来的Array push一个元素
    let rst1  = a1.arrayByAddingObject(item)
    rst1[1] as? Person == a1.objectAtIndex(1) as? Person // 浅拷贝
    
    // 相当于 浅拷贝出来的Array push一个数组
    a1.arrayByAddingObjectsFromArray(arr)
    
    // 通过 range 得到浅拷贝数组
    a1.subarrayWithRange(NSRange(0...0))
    
    // 谓词过滤
    let p1 = NSPredicate { (item, _) -> Bool in
        if let item = item as? Person where item.age > 10 {
            return true
        }
        return false
    }
    a1.filteredArrayUsingPredicate(p1).count
    
}

// 排序
func sorting(){
    let a1 = NSArray(objects: Person("a", 10), Person("b", 20), Person("c", 30))
   
    // opt 可指定并行
    a1.sortedArrayWithOptions(.Concurrent) { (prev, next) -> NSComparisonResult in
        let p1 = prev as! Person
        let p2 = next as! Person
        if p1.age > p2.age {
            return NSComparisonResult.OrderedAscending
        }else{
            return NSComparisonResult.OrderedDescending
        }
    }
    
}


```