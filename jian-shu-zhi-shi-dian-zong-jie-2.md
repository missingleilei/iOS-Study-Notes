#### 1.SDWebImage源码浅析

[https://www.jianshu.com/p/05b6e70f030b](https://www.jianshu.com/p/05b6e70f030b)

#### 2.UITextView根据输入文字实时改变高度

```
  - (float) heightForTextView: (UITextView*)textView WithText: (NSString*) strText{
        CGSize constraint = CGSizeMake(textView.contentSize.width , CGFLOAT_MAX);
        CGRect size = [strText boundingRectWithSize:constraint options:(NSStringDrawingUsesLineFragmentOrigin|
        NSStringDrawingUsesFontLeading)attributes:@{NSFontAttributeName: [UIFont systemFontOfSize:14]} context:nil];
        float textHeight = size.size.height + 22.0;
        return
        textHeight;
  }

  - (BOOL)textView:(UITextView *)textView shouldChangeTextInRange:(NSRange)range replacementText:(NSString *)text
{
    CGRect frame = textView.frame;
    float height;
    if ([text isEqual:@""]) {

        if (![textView.text isEqualToString:@""]) {

            height = [ self heightForTextView:textView WithText:[textView.text substringToIndex:[textView.text length] - 1]];

        }else{

            height = [ self heightForTextView:textView WithText:textView.text];
        }
    }else{

            height = [self heightForTextView:textView WithText:[NSString stringWithFormat:@"%@%@",textView.text,text]];
    }

    frame.size.height = height;
    [UIView animateWithDuration:0.5 animations:^{

            textView.frame = frame;

        } completion:nil];

    return YES;
}
```

#### 3.Cell中嵌套Cell

[https://www.jianshu.com/p/01ccb02bbffd](https://www.jianshu.com/p/01ccb02bbffd)

[https://www.jianshu.com/p/ee729c4d509c](https://www.jianshu.com/p/ee729c4d509c)

#### 4.iOS小知识点

https://www.jianshu.com/p/1ff9e44ccc78



