---
title: CATransition 转场动画
categories: [iOS 动画]
published: true
topmost: false
---

present 动画

```objc
    CATransition *animation = [CATransition animation];
    animation.type = kCATransitionPush;//设置动画的类型
    animation.subtype = kCATransitionFromRight; //设置动画的方向
    animation.timingFunction = [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseInEaseOut];
    animation.duration = 0.5f;
    [self.view.window.layer addAnimation:animation forKey:nil];
    [self presentViewController:vc animated:NO completion:nil];
```

dismiss 动画

```objc
    CATransition *animation = [CATransition animation];
    animation.type = kCATransitionPush;//设置动画的类型
    animation.subtype = kCATransitionFromLeft; //设置动画的方向
    animation.timingFunction = [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseInEaseOut];
    animation.duration = 0.5f;
    [self.view.window.layer addAnimation:animation forKey:nil];
    [self dismissViewControllerAnimated:NO completion:nil];
```
