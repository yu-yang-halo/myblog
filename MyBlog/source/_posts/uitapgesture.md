---
title: iOS手势单双击冲突问题
date: 2016-09-30 15:06:34
tags: iOS开发
---
 
### 官方文档这样写道
<font color=#0099ff size=3>
  create a relationship with another gesture recognizer that will prevent this gesture's actions from being called until otherGestureRecognizer transitions to UIGestureRecognizerStateFailed if otherGestureRecognizer transitions to UIGestureRecognizerStateRecognized or UIGestureRecognizerStateBegan then this recognizer will instead transition to UIGestureRecognizerStateFailed example::a single tap may require a double tap to fail
</font>

<font color=#ff0000 size=3>
  -(void)requireGestureRecognizerToFail:(UIGestureRecognizer *)otherGestureRecognizer;
  例如：
   [singleGestureRecognizer requireGestureRecognizerToFail:doubleGestureRecognizer]
 </font>

<font color=#0099ff size=3>
  
  大概的意思就是一个手势与另一个手势需要保持一种关系，只有当另外一个手势失败了当前手势才能调用。
  反之如果另外的手势被被识别了，当前手势将不会被调用。
  所以单击手势必须等待双击手势的识别结果才不会导致冲突问题。
 
</font>