# NSSet

```swift


// #1 Set构造
func instanceSet(){
    // 作用？
    // Set()
    // Set(sequence: S)
    // Set(minimumCapacity: Int)
    
    // 逐一指定元素，不需要同一类型
    Set(arrayLiteral: 1, 2, 3, "a")
}

// #2 NSSet构造
func instanceNSSet(){
    NSSet(array: [1, 2, 3])
    NSSet(arrayLiteral: 1, 2, 3)
    NSSet(object: 1)
    NSSet(objects: 1, 2, 3)
    NSSet(set: Set(arrayLiteral: 1,2,3))
}


// #3 访问成员
func accessMembers(){
    let set = NSSet(objects: "2", true, 0, 2)

    // set -> array
    set.allObjects
    
    // 返回其中一个成员或 nil
    set.anyObject()
    
    // 是否包含成员
    set.containsObject(1)
    
    // NSPredicate @TODO
    // set.filteredSetUsingPredicate(predicate: NSPredicate)
    
    // 返回其中一个成员或 nil
    set.member(true)
    
    // NSEnumerator @TODO
    // set.objectEnumerator()
    
    // 迭代每一个成员
    set.enumerateObjectsUsingBlock { (item, whatTheHell) -> Void in
        print(item)
    }
    
    // 迭代每一个成员 (并发执行)
    set.enumerateObjectsWithOptions(NSEnumerationOptions.Concurrent) { (item, whatTheHell) -> Void in
        print(item)
    }
    
    // 返回通过测试的新Set 类似数组的filter
    set.objectsPassingTest { (item, whatTheHell) -> Bool in
        if let val = item as? Int {
            if val > 0 {
                return true
            }
        }
        return false
    }
    
    // 返回通过测试(并发执行)的新Set
    set.objectsWithOptions(NSEnumerationOptions.Concurrent) { (item, whatTheHell) -> Bool in
        if let val = item as? Int{
            if val > 0 {
                return true
            }
        }
        return false
    }
}

// #4 与 Set 对比
func compareSet(){
    let set = NSSet(arrayLiteral: 1, 2)
    
    // 是否子集
    let set1 = Set(arrayLiteral: 1, 2, 3, 4)
    set.isSubsetOfSet(set1)
    
    // 是否交集
    let set2 = Set(arrayLiteral: 5)
    let set3 = Set(arrayLiteral: 1, 5)
    set.intersectsSet(set2)
    set.intersectsSet(set3)
    
    // 是否 equal
    let set0 = Set(arrayLiteral: 1, 2)
    set.isEqualToSet(set0)
    
    
    // 求值中断 signal SIGABRT，playground 沙箱限制了哪些？
    // set.setValue(true, forKey: "key1")
    // set.valueForKey("key1")
    
}

// #5 NSSortDescriptor @TODO
func createSortedArray(){
    let set = NSSet(arrayLiteral: 1, 2)
//    set.sortedArrayUsingDescriptors(sortDescriptors: [NSSortDescriptor])
}


// #6 键值观察 @TODO
func kvObserving(){
    let set = NSSet(arrayLiteral: 1, 2)
//    set.addObserver(<#T##observer: NSObject##NSObject#>, forKeyPath: <#T##String#>, options: <#T##NSKeyValueObservingOptions#>, context: <#T##UnsafeMutablePointer<Void>#>)
    
}


// #7 对象字符串表达
func description(){
    let set = NSSet(arrayLiteral: 1, 2)
    set.description
}




```