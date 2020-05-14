---
title: tableView 实现单选可取消
categories: [TableView]
published: true
topmost: false
---

1. 设置 self.allowsMultipleSelection = YES;
2. 在 willSelectRowAtIndexPath 添加取消逻辑

```objc
- (NSIndexPath *)tableView:(UITableView *)tableView willSelectRowAtIndexPath:(NSIndexPath *)indexPath {
    //实现单选取消逻辑
    NSArray *indexPaths = [tableView indexPathsForSelectedRows];
    if (indexPaths.count > 0) {
        for (NSIndexPath *indexPath in indexPaths) {
            [tableView deselectRowAtIndexPath:indexPath animated:YES];
        }
    }
    return indexPath;
}
```
