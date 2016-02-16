# NSString

```swift
func exec(){
//    getStringLength()
//    getChars()
//    combining()
//    dividing()
//    finding()
//    replacing()
//    comparing()
//    changingCase()
//    numericValues()
//    workingWithEncodings()
//    workingWithPaths()
    workingWithURLs()
}
exec()

/*
    #DataType
    * typealias unichar = UInt16
    * NSStringCompareOptions
    * NSStringEncodingConversionOptions
    * NSStringEncoding
*/

/*
    #Constants
    * NSStringCompareOptions
    * NSStringEncodingConversionOptions
    * <NSUTF8StringEncoding>

    * NSMaximumStringLength
    * NSStringEnumerationOptions
    * <handle exception>
        * NSCharacterConversionException
        * NSParseErrorException
*/

// # Linguistic Tagging and Analysis @TODO

// # Working with URLs
func workingWithURLs(){
    // 对 URLComponents 的 %编码 和%解码
    // ... URLComponents 源字符串
    let schema = "http"
    let _user = "?用户名"
    let _pwd = "??密码"
    let _host = "www.啊.com"
    let _path = "/section/第一章.php"
    let _query = "key1=true&key2=http://主机名/资源路径?参数#锚点"
    let _fragment = "!锚点1"
    
    let TOCKEN_SCHEMA = "://"
    let TOCKEN_PWD = ":"
    let TOCKEN_AUTHORITY = "@"
    let TOCKEN_QUERY = "?"
    let TOCKEN_FRAGMENT = "#"

    // ... %编码
    let host = NSString(string: _host).stringByAddingPercentEncodingWithAllowedCharacters(NSCharacterSet.URLHostAllowedCharacterSet())!
    let user = NSString(string: _user).stringByAddingPercentEncodingWithAllowedCharacters(NSCharacterSet.URLUserAllowedCharacterSet())!
    let pwd = NSString(string: _pwd).stringByAddingPercentEncodingWithAllowedCharacters(NSCharacterSet.URLPasswordAllowedCharacterSet())!
    let path = NSString(string: _path).stringByAddingPercentEncodingWithAllowedCharacters(NSCharacterSet.URLPathAllowedCharacterSet())!
    let query = NSString(string: _query).stringByAddingPercentEncodingWithAllowedCharacters(NSCharacterSet.URLQueryAllowedCharacterSet())!
    let fragment = NSString(string: _fragment).stringByAddingPercentEncodingWithAllowedCharacters(NSCharacterSet.URLFragmentAllowedCharacterSet())!
    let urlstr = schema + TOCKEN_SCHEMA + user + TOCKEN_PWD + pwd + TOCKEN_AUTHORITY + host + path + TOCKEN_QUERY + query + TOCKEN_FRAGMENT + fragment
    
    // ... %解码
    let url = NSURL(string: urlstr)!
    let decodeQuery = NSString(string: url.query!).stringByRemovingPercentEncoding!
    let decodeUser = NSString(string: url.user!).stringByRemovingPercentEncoding!
    print(decodeUser, decodeQuery)
}

// # Working with Paths @TODO-LvH
func workingWithPaths(){
    let str = NSString(string: "~/Documents/Section1/README.md")
    [str.pathComponents[0], str.pathComponents[1]]
}

// # Working with Encodings @TODO-LvH
func workingWithEncodings(){
    let str = NSString(string: "a")
}


// # Getting Numeric Values
func numericValues(){
    let str = NSString(string: "10e3")
    str.doubleValue
}

// # Getting Strings and Mapping @TODO

// # Changing case
func changingCase(){
    let str = NSString(string: "bcd aaa")
    str.capitalizedString
    str.uppercaseString
    str.uppercaseString.lowercaseString
}

// # Getting a Shared Prefix @TODO
    // - commonPrefixWithString:options:

// # Folding Strngs @TODO
    // - stringByFoldingWithOptions:locale:

// # Identifying and Comparing Strinngs
func comparing(){
    let str = NSString(string: "bcd aaa")
    
    // 比较，约束：无视大小写
    let rst1 = str.caseInsensitiveCompare("Bcd AAA")
    switch rst1{
    case .OrderedSame:
        print("the same")
    case .OrderedAscending:
        print("origin is smaller")
    case .OrderedDescending:
        print("origin is bigger")
    }
    
    // 比较，约束：options, range
    // @TODO locale
    switch str.compare("BCD", options: NSStringCompareOptions.CaseInsensitiveSearch, range: NSRange(location: 0, length: 3), locale: nil) {
    case .OrderedSame:
        print("the same")
    case .OrderedAscending:
        print("origin is smaller")
    case .OrderedDescending:
        print("origin is bigger")
    }
    
    // 是否包含前缀；尾缀；是否相等
    str.hasPrefix("b")
    str.hasSuffix("a")
    
    // 字符串对象通过 hash 属性来判断是否相等
    str.hash
    "bcd aaa".hash
    str == "bcd aaa"
}

// # Converting String Contents Into a Property List
func proplist(){
    // @Q
    // let str = NSString(string: " ab ac ab")
    // str.propertyList()
}

// # Determining Composed CharSequences @TODO

// # Determining Line and Paragraph Ranges @TODO

// # Replacing Substrings
func replacing(){
    let str = NSString(string: " ab ac ab")
    
    str.stringByReplacingOccurrencesOfString("ab", withString: "_")
    str.stringByReplacingOccurrencesOfString("ab", withString: "_", options: NSStringCompareOptions.BackwardsSearch, range: NSRange(location: 0, length: 4))
    str.stringByReplacingCharactersInRange(NSRange(location: 0, length: 3), withString: "_")
}

// # Finding Chars and Substrings
func finding(){
    let str = NSString(string: " ab ac ad")

    // ... groupA
    let range1 = str.rangeOfCharacterFromSet(NSCharacterSet.alphanumericCharacterSet())
    [range1.location, range1.length]
    
    let range2 = str.rangeOfCharacterFromSet(NSCharacterSet.alphanumericCharacterSet(), options: NSStringCompareOptions.BackwardsSearch)
    [range2.location, range2.length]
    
    let range3 = str.rangeOfCharacterFromSet(NSCharacterSet.alphanumericCharacterSet(), options: NSStringCompareOptions.BackwardsSearch, range: NSRange(location: 0, length: 1))
    [range3.location, range3.length]
    
    // ... groupB
    let range4 = str.rangeOfString("ab")
    [range4.location, range4.length]
    
    let range5 = str.rangeOfString("ab", options: NSStringCompareOptions.BackwardsSearch)
    [range5.location, range5.length]
    
    let range6 = str.rangeOfString("ac", options: NSStringCompareOptions.BackwardsSearch, range: NSRange(location: 0, length: 3))
    [range6.location, range6.length]
    
    // @TODO NSLocal
    // - rangeOfString:options:range:locale:
    
    // @TODO    
    // - enumerateLinesUsingBlock:
    // - enumerateSubstringsInRange:options:usingBlock:

}


// # Dividing Strings
func dividing(){
    let str = NSString(string: "a| b| cd")
    
    str.componentsSeparatedByString("|")
    str.componentsSeparatedByCharactersInSet(NSCharacterSet.whitespaceAndNewlineCharacterSet())
    str.stringByTrimmingCharactersInSet(NSCharacterSet.alphanumericCharacterSet())
    str.substringFromIndex(1)
    str.substringWithRange(NSRange(location: 1, length: 3))
    str.substringToIndex(5)
}

// # Combining Strings
func combining(){
    let str = NSString(string: "一字符串")
    // 字符串的+运算
    str.stringByAppendingString("。")
    
    // 用变量字符串替代 "\(identify)"
    let user = "adi"
    let pwd = "xxoxx"
    str.stringByAppendingFormat(" user = %@ & pwd = %@", user, pwd)
    
    // 字符串截取 或字符串扩展
    str.stringByPaddingToLength(10, withString: "$%", startingAtIndex: 0)
    str.stringByPaddingToLength(10, withString: "$%", startingAtIndex: 1)
}


// # Getting C String @TODO

// # Getting Chars ang Bytes
func getChars(){
    let str = NSString(string: "a一字符串")
    
    // 返回 unichar(UInt16)
    str.characterAtIndex(0)
    
    // @TODO
    // - getCharacters:range:
    // - getBytes:maxLength:usedLength:encoding:options:range:remainingRange:
}

// # Getting String's length
func getStringLength(){
    let str = NSString(string: "一字符串")
    
    // unicode 字符数量
    str.length
    
    // 指定存储编码下的字符长度
    str.lengthOfBytesUsingEncoding(NSUTF16StringEncoding)
    
    // @Q
    str.maximumLengthOfBytesUsingEncoding(NSUTF16StringEncoding)
}

```