# NSDate

```swift
func exec(){
//    instance()
//    temporalBounds()
//    compare()
//    gettingIntervals()
//    calc()
    representing()
}
exec()

/*
#DataType
* typealias NSTimeInterval = Double

*/

// #时间的字符串表示
func representing(){
    let d1 = NSDate()
    d1.description
    d1.descriptionWithLocale(NSLocale.currentLocale())
}

// #时间运算
func calc(){
    let d1 = NSDate()
    let d2 = NSDate(timeInterval: -60, sinceDate: d1)
    
    d2.dateByAddingTimeInterval(-60)
}

// #计算时间距离(秒)
func gettingIntervals(){
    let d1 = NSDate()
    let d2 = NSDate(timeInterval: -60, sinceDate: d1)
    
    d2.timeIntervalSinceNow
    d2.timeIntervalSinceReferenceDate
    d2.timeIntervalSince1970
    d2.timeIntervalSinceDate(d1)
    NSDate.timeIntervalSinceReferenceDate()
}

// #时间对比
func compare(){
    let d1 = NSDate()
    let d2 = NSDate(timeInterval: -60, sinceDate: d1)
    
    d1.isEqualToDate(d2)
    d1.earlierDate(d2)
    d1.laterDate(d2)
    switch d1.compare(d2) {
    case .OrderedSame:
        "d1 = d2"
    case .OrderedAscending:
        "d1 < d2"
    case .OrderedDescending:
        "d1 > d2"
    }
}

// #时间边界
func temporalBounds(){
    NSDate.distantFuture()
    NSDate.distantPast()
}

// #构造
func instance(){
    let SECS:NSTimeInterval = 365 * 24 * 3600
    
    var d = NSDate()
    d = NSDate(timeIntervalSinceReferenceDate: SECS)
    d = NSDate(timeIntervalSinceNow: -SECS)
    d = NSDate(timeIntervalSince1970: SECS)
    d = NSDate(timeInterval: SECS, sinceDate: d)
}
```
