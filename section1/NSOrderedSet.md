# NSOrderedSet

```swift
func exec(){
//    instance()
//    accessMembers()
//    compare()
    createSortedArray()
//    convertCollection()
//    filter()
//    predicate()
}
exec()


// # prepare
// 深拷贝的对象，需实现NSCopying的copyWithZone拷贝方法
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
        // @TODO NSZone
        return Person(self.name)
    }
}

func printPersonName(FromFirstItemOfSet set:NSOrderedSet) -> String?{
    if let item = set.firstObject as? Person {
        return item.name
    }
    return nil
}

// # 构造
func instance(){

    /*
      使用 Array类型来构造
      -init(array:)
      -init(array: copyItems:) 是否深拷贝，默认 false
      -init(array: range: copyItems:) 指定构造Set的长度
    */
    let set1 = NSOrderedSet(array: [Person("a"), Person("b")], range: NSRange(0...1), copyItems: false)

    // 测试 深/浅拷贝
    let set2 = NSOrderedSet(orderedSet: set1, copyItems: true)
    (set1.firstObject as? Person)?.name = "12344"
    printPersonName(FromFirstItemOfSet: set1)
    printPersonName(FromFirstItemOfSet: set2)
    
    /*
      使用 NSOrderedSet类型来构造
      同上，一样有三个构造器
    */
    NSOrderedSet(orderedSet: set1, range: NSRange(0...0), copyItems: false)
    
    
    /*
      使用 Set类型来构造
      两个构造器
    */
    NSOrderedSet(set: Set<Person>() )
    NSOrderedSet(set: Set<Person>(), copyItems: false)
    
    
    /*
      使用 AnyObject类型来构造
    */
    NSOrderedSet(object: Person("a"))
    NSOrderedSet(objects: Person("a"), Person("b"))
    // NSOrderedSet(objects: UnsafePointer<AnyObject?>, count: Int) // @Q
}


// # 访问成员
func accessMembers(){
    let p1 = Person("a")
    let p2 = Person("b")
    let p3 = Person("c")
    let set = NSOrderedSet(objects: p1, p2, p3)
    let indexSet = NSIndexSet(indexesInRange: NSRange(0...1))
    
    // 属性
    set.firstObject
    set.lastObject
    set.reversedOrderedSet
    
    // 是否包含成员
    set.containsObject(p1)
    
    // 迭代
    // NSEnumerationOptions 可选参数，指定并行或倒序迭代
    set.enumerateObjectsWithOptions(NSEnumerationOptions.Concurrent){ (item, index, stop) -> Void in
        if index == 1 {
            (item as? Person)?.name
        }else{
            print(1)
        }
        // @TODO stop 如何使用
    }
    
    // set[0] // @Q subscript failed?
    
    
    /*
      成员与索引
      index -> AnyObject
      AnyObject -> index
      NSIndexSet -> [AnyObject]
    */
    set.objectAtIndex(0)  // 通过索引获取成员
    set.indexOfObject(p2) // 通过成员获取索引
    set.objectsAtIndexes(NSIndexSet(index: 1))
    set.objectsAtIndexes(NSIndexSet(indexesInRange: NSRange(0...1)))
    

    /*
      passingTest等约束来获取索引
      passingTest -> index
      options, passingTest -> index
      NSIndexSet, options, passingTest -> index
    */
    set.indexOfObjectAtIndexes(indexSet, options: NSEnumerationOptions.Reverse) { (item, index, stop) -> Bool in
        if let item = item as? Person where item.name == "a" {
            return true
        }
        return false
    }
    
    
    /*
      passingTest等约束来获取索引集合
      passingTest -> NSIndexSet
      options, passingTest -> NSIndexSet
      NSIndexSet, options, passingTest -> NSIndexSet
    */
    set.indexesOfObjectsAtIndexes(indexSet, options: NSEnumerationOptions.Concurrent) { (item, index, stop) -> Bool in
        return true
    }
    
    // @TODO Enumerator
    set.objectEnumerator()
    
    
    // @Q
    // indexOfObject(_:inSortedRange:options:usingComparator:)

}

// # 比较: equal 交集 子集
func compare(){
    let p1 = Person("a")
    let p2 = Person("b")
    let p3 = Person("c")
    let set1 = NSOrderedSet(objects: p1, p2, p3)
    let set2 = NSOrderedSet(objects: p1, p2, p3)
    let set3 = NSOrderedSet(objects: p1, p2)
    let set = Set(arrayLiteral: p1, p2, p3)
    
    // 与 NSOrderedSet
    set1.isEqualToOrderedSet(set2)
    set3.intersectsOrderedSet(set1)
    set3.isSubsetOfOrderedSet(set1)
    
    // 与 Set
    set3.intersectsSet(set)
    set3.isSubsetOfSet(set)
}


// # 返回新排序的数组
func createSortedArray(){
    let set1 = NSOrderedSet(objects: Person("p2"), Person("p3"), Person("p1"))
    
    /*
      @TODO
      NSSortDescriptor
      NSComparator
    */
    //  set1.sortedArrayUsingDescriptors(<#T##sortDescriptors: [NSSortDescriptor]##[NSSortDescriptor]#>)
    //  set1.sortedArrayUsingComparator(<#T##cmptr: NSComparator##NSComparator##(AnyObject, AnyObject) -> NSComparisonResult#>)
    
}

// # 转换成其他 Collection类
func convertCollection(){
    let set1 = NSOrderedSet(objects: Person("p2"), Person("p3"), Person("p1"))
    set1.set
    set1.array
}

// # 过滤
func filter(){
    let p1 = Person("a")
    let p2 = Person("b")
    let p3 = Person("c")
    let set = NSOrderedSet(objects: p1, p2, p3)
    
    let result = set.filteredOrderedSetUsingPredicate(NSPredicate(block: { (person, dict) -> Bool in
        if let person = person as? Person where person.name == "a" {
            return true
        }
        return false
    }))
    (result.firstObject as? Person)?.name
    
}


// # 谓词
func predicate(){
    let employee = Person("adi", 100)
    let valueMap: [String:Int] = ["age": 110, "rest": -10]
    
    let p1 = NSPredicate(format: "age > 24")
    let p2 = NSPredicate(format: "age > %@", NSNumber(int: 24))
    let p3 = NSPredicate(format: "%K > %@", "age", NSNumber(int: 24))
    let p4 = NSPredicate(format: "age > $age")
    let p5 = NSPredicate { (item, valueMap) -> Bool in
        if let item = item as? Person where item.age > 10 {
            return true
        }
        return false
    }
    
    // exec p1~p3
    if p3.evaluateWithObject(employee) {
        1
    } else {
        2
    }
    
    // exec p4
    if p4.evaluateWithObject(employee, substitutionVariables: valueMap) {
        1
    } else {
        2
    }
    
    // apply p5
    let set = NSSet(objects: Person("e1", 10), Person("e2", 20))
    set.filteredSetUsingPredicate(p5).count
    
    
}

```