---
title: Cell animation stop fraction must be greater than start fraction
tags: 
    -iOS开发
categories: iOS
---

### Cell animation stop fraction must be greater than start fraction 问题

``` object-c
-(CGFloat)tableView:(UITableView *)tableView heightForFooterInSection:(NSInteger)section {
   //return 0.001f;设置大于0的数导致的crash
   return 0;
}
```

#### 可以解决crash的bug,这应该是苹果本身的一个bug，mark一下
