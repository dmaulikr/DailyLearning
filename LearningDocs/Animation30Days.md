# iOSAnimation30Days.md

## 基础动画

###  UIViewAnimationOptions

```
typedef NS_OPTIONS(NSUInteger, UIViewAnimationOptions) {
    UIViewAnimationOptionLayoutSubviews            = 1 <<  0,
    UIViewAnimationOptionAllowUserInteraction      = 1 <<  1, // turn on user interaction while animating
    UIViewAnimationOptionBeginFromCurrentState     = 1 <<  2, // start all views from current value, not initial value
    UIViewAnimationOptionRepeat                    = 1 <<  3, // repeat animation indefinitely
    UIViewAnimationOptionAutoreverse               = 1 <<  4, // if repeat, run animation back and forth//需要配合UIViewAnimationOptionRepeat来使用
    UIViewAnimationOptionOverrideInheritedDuration = 1 <<  5, // ignore nested duration
    UIViewAnimationOptionOverrideInheritedCurve    = 1 <<  6, // ignore nested curve
    UIViewAnimationOptionAllowAnimatedContent      = 1 <<  7, // animate contents (applies to transitions only)
    UIViewAnimationOptionShowHideTransitionViews   = 1 <<  8, // flip to/from hidden state instead of adding/removing
    UIViewAnimationOptionOverrideInheritedOptions  = 1 <<  9, // do not inherit any options or animation type
    
    UIViewAnimationOptionCurveEaseInOut            = 0 << 16, // default//start的时候加速 & 快结束的时候减速
    UIViewAnimationOptionCurveEaseIn               = 1 << 16,//start的时候加速
    UIViewAnimationOptionCurveEaseOut              = 2 << 16,//快结束的时候减速
    UIViewAnimationOptionCurveLinear               = 3 << 16,//没有加减速
    
    UIViewAnimationOptionTransitionNone            = 0 << 20, // default
    UIViewAnimationOptionTransitionFlipFromLeft    = 1 << 20,
    UIViewAnimationOptionTransitionFlipFromRight   = 2 << 20,
    UIViewAnimationOptionTransitionCurlUp          = 3 << 20,
    UIViewAnimationOptionTransitionCurlDown        = 4 << 20,
    UIViewAnimationOptionTransitionCrossDissolve   = 5 << 20,
    UIViewAnimationOptionTransitionFlipFromTop     = 6 << 20,
    UIViewAnimationOptionTransitionFlipFromBottom  = 7 << 20,
} NS_ENUM_AVAILABLE_IOS(4_0);

```

Example:

```
- (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
    _headingLb.centerX = 0;
    [UIView animateWithDuration:1 delay:0.4 options:(UIViewAnimationOptionRepeat |  UIViewAnimationOptionAutoreverse | UIViewAnimationOptionCurveLinear) animations:^{
        _headingLb.centerX += self.view.width;
    } completion:^(BOOL finished) {
        NSLog(@"animation has finish.");
    }];
}
```


### 弹簧效果

```
UIView.animate(withDuration:delay:usingSpringWithDamping:initialSpringVelocity:optio ns:animations:completion:). 
```

```
UIView.animate(withDuration: 0.5, delay: 0.5,usingSpringWithDamping: 0.5, initialSpringVelocity: 0.0, options: [],animations: {  self.loginButton.center.y -= 30.0  self.loginButton.alpha = 1.0}, completion: nil)
```

```
• usingSpringWithDamping: This controls the amount of damping, or reduction, applied to the animation as it approaches its final state. This parameter accepts values between 0.0 and 1.0. Values closer to 0.0 create a bouncier animation, while values closer to 1.0 create a stiff-looking effect. You can think of this value as the “stiffness” of the spring.• initialSpringVelocity: This controls the initial velocity of the animation. A value of 1.0 sets the velocity of the animation to cover the total distance of the animation in the span of one second. Bigger and smaller values will cause the animation to have more or less velocity.
```


![Sortable. drag and reorder the blocks.](https://raw.githubusercontent.com/fangwei716/ThirtyDaysOfReactNative/screenshots/screenshot/day18.gif)


![A tinder swipe](https://raw.githubusercontent.com/fangwei716/ThirtyDaysOfReactNative/screenshots/screenshot/day14.gif)


![Fuzzy search](https://raw.githubusercontent.com/fangwei716/ThirtyDaysOfReactNative/screenshots/screenshot/day17.gif)

### Transitions
Transitions：过渡，转变，利用一些数学矩阵进行变换。

> 之前的2章，主要通过`UIView`的一些动画属性，比如`position`, `alpha`来实现。下面将用`变换`来实现.


### Keyframe Animations

![](http://oc98nass3.bkt.clouddn.com/2017-07-09-14995744576755.jpg)

大多数情况下，您会发现需要为视图设置多个连续的动画。在此之前，您已经使用完成闭包将多个动画连接到一起了。
这种方法在两个简单动画的链接中起作用，但当你想将三个、四个或更多的动画组合在一起时，可能会导致一些令人难以置信的混乱和复杂的代码。


```
UIView.animate(withDuration: 0.5,  animations: {    view.center.x += 200.0  },  completion: { _ in    UIView.animate(withDuration: 0.5,      animations: {
      view.center.y += 100.0      },      completion: { _ in        UIView.animate(withDuration: 0.5,          animations: {            view.center.x -= 200.0          },          completion: { _ in            UIView.animate(withDuration: 0.5,              animations: {                view.center.y -= 100.0              }         ) }      ) }    ) })
```


## 参考

1. [fangwei716/30-days-of-react-native](https://github.com/fangwei716/30-days-of-react-native)
2. [100 Days of Swift](http://samvlu.com/) 
3. [30DaysofSwift](https://github.com/allenwong/30DaysofSwift)
4. [iOS_Animations_by_Tutorials_v3.1](https://store.raywenderlich.com/products/ios-animations-by-tutorials)




