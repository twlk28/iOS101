# NSRange


## 定义
struct NSRange {
	Int location;
	Int length;
}

## 常用方法

### 1.比较两个`NSRange`是否值相同
```swift
func NSEqualRanges(_ range1: NSRange, _ range2: NSRange2) -> Bool
```

### 2. 交集
```swift
func NSIntersectionRange(_ range1: NSRange, _ range2: NSRange) -> NSRange
```

* 相关：NSUnionRange


### 3. 命中测试
```swift
func NSLocationInRange(_ loc: Int, _ range: NSRange) -> Bool
```


### 4. 创建`NSRange` 默认构造
```swift
func NSMakeRange(_ loc: Int, _ len: Int) -> NSRange
```


### 5. 计算`NSRange` 的最大`location`
```swift
func NSMaxRange(_ range: NSRange) -> Int
```


### 6. 创建`NSRange` 字符串表达
```swift
func NSRangeFromString(_ aString: String) -> String
```
* 相关: NSStringFromRange


### 7. NSStringFromRange
```swift
func NSStringFromRange(_ range: NSRange) -> String
```

* 返回 "{a,b}" ; a b 非负

### 8. 并集
```swift
func NSUnionRange(_ range1: NSRange, _ range2: NSRange) -> NSRange
```














