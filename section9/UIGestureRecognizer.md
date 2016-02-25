# UIGestureRecognizer

## 类型概述
1. UIGestureRecognizer
	* 构造 `init:target:action` 
		* action: 响应该手势的方法
		* target: 响应该手势的方法所在的对象，为 UIView子类 或 ViewController子类
	* 手势的状态 `UIGestureRecognizerState`
		* one of `Possible Began Changed Ended Cancelled Failed Recognized`
2. UIGestureRecognizer 的子类
	* `Tap` 
		* `numberOfTapsRequired` 构造时可指定，表示tap多少次，才作为可识别的手势
		* `numberOfTouchesRequired` 构造时可指定，表示touch的手指数为多少时，才作为可识别的手势
	* `Pinch` 两手指伸缩
		* `scale` CGFloat，伸缩大小比率
		* `velocity` CGFloat，伸缩速率 
	* `Rotation` 
		* `rotation` CGFloat，`.Began ~ .End `旋转的角度
		* `velocity` CGFloat，旋转速率 
	* `Swipe`
		* `numberOfTouchesRequired` 构造时可指定，表示touch的手指数为多少时，才作为可识别的手势
		* `direction` one of `Up Down Left Right`
		* @Q 如何实现 swipe到某个距离执行函数(调整UI)
	* `Pan`
		* `maximumNumberOfTouches` 构造时可指定，表示touch的手指数小于该值时，才作为可识别的手势
		* `minimumNumberOfTouches` 构造时可指定，功能类似上面
		* `velocityInView:` // @Q 为啥需要传个 view进来，作用；为啥的是方法不是向上面一个属性
		* `translationInView:` 
		* `setTranslation:inView:`
	* `LongPress`
		* `minimunPressDuration`
		* `numberOfTouchesRequired`
		* `numberOfTapsRequired`
		* `allowableMovement`

## 响应手势
1. 视图添加手势识别 `UIView.addGestureRecognizer:`
2. 视图 或 `ViewController` 实现 手势识别的回调方法，即对应`UIGestureRecognizer`的`action`


## 解决手势冲突
* `UIGestureRecognizer` 实例的 `requireGestureRecognizerToFail`
* 或者实现 `UIGestureRecognizerDelegate`的`gestureRecognizer:shouldRequireFailureOfGestureRecognizer:`

### Demo
```swift
import UIKit

class ViewController: UIViewController, UIGestureRecognizerDelegate {

    override func viewDidLoad() {
        super.viewDidLoad()
        
        let tapGesture = UITapGestureRecognizer(target: self, action: "tap1:")
        view.addGestureRecognizer(tapGesture)
        
        let tapGesture2 = UITapGestureRecognizer(target: self, action: "tap2:")
        tapGesture2.numberOfTapsRequired = 2
        view.addGestureRecognizer(tapGesture2)

//        #1 设置手势识别优先级
//        tapGesture.requireGestureRecognizerToFail(tapGesture2)
        
        tapGesture.delegate = self
        tapGesture2.delegate = self
    }

    
    func tap1(gesture: UITapGestureRecognizer) {
        switch gesture.state {
        case .Recognized:
            print("tap1: ", gesture.state)
        default:
            print("~")
        }
    }
    
    func tap2(gesture: UITapGestureRecognizer) {
        switch gesture.state {
        case .Recognized:
            print("tap2: ", gesture.state)
        default:
            print("~")
        }
    }
    
    
    // MARK: delegate for UIGestureRecognizer
    
//    #2 第二种手势识别优先级
//    func gestureRecognizer(gestureRecognizer: UIGestureRecognizer, shouldRequireFailureOfGestureRecognizer otherGestureRecognizer: UIGestureRecognizer) -> Bool {
//        if let gestureRecognizer = gestureRecognizer as? UITapGestureRecognizer, otherGestureRecognizer = otherGestureRecognizer as? UITapGestureRecognizer {
//                if gestureRecognizer.numberOfTapsRequired == 1 && otherGestureRecognizer.numberOfTapsRequired == 2 {
//                    return true
//                }
//        }
//        return false
//    }
//    #2 第二种手势识别优先级 除了参数倒过来 有啥区别？
    func gestureRecognizer(gestureRecognizer: UIGestureRecognizer, shouldBeRequiredToFailByGestureRecognizer otherGestureRecognizer: UIGestureRecognizer) -> Bool {
        if let gestureRecognizer = gestureRecognizer as? UITapGestureRecognizer, otherGestureRecognizer = otherGestureRecognizer as? UITapGestureRecognizer {
            if gestureRecognizer.numberOfTapsRequired == 2 && otherGestureRecognizer.numberOfTapsRequired == 1 {
                return true
            }
        }
        return false
    }

}
```

## ref
* [1][1]

--
[1]: http://www.arkulo.com/2015/01/05/gesture/
