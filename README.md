# TableRefreshAnimation

## Introduction
TableView pull down or pull up refresh animation

## How to use
```obejct-c
- (void)viewWillAppear:(BOOL)animated {
    [super viewWillAppear:animated];

    self.refreshHeaderView = [[TableHeaderRefreshView alloc] initWithScrollView:self.refreshTableView hasNavigationBar:YES];

    [self.refreshHeaderView doRefresh];

    __weak __typeof(&*self)weakSelf = self;
    [self.refreshHeaderView addRefreshingBlock:^{
        // you can do some net request or other refresh operation
        // ...

        // here simulate do some refresh operation,and after 3s refresh complate
        double delayTime = 3.0;
        dispatch_time_t time = dispatch_time(DISPATCH_TIME_NOW, delayTime * NSEC_PER_SEC);
        dispatch_after(time, dispatch_get_main_queue(), ^{
        [weakSelf.refreshHeaderView stopRefresh];
        });
    }];

    self.refreshFooterView = [[TableFooterRefreshView alloc] initWithScrollView:self.refreshTableView hasNavigationBar:YES];

    [self.refreshFooterView addRefreshingBlock:^{
        // you can do some net request or other refresh operation
        // ...

        // here simulate do some refresh operation,and after 3s refresh complate
        NSUInteger count = weakSelf.dataSource.count;
        if (count <= kMaxDataSourceCount) {
        for (NSUInteger i = count; i < count + 10; i++) {
        NSString *data = [NSString stringWithFormat:@"This is title %lu",i];
            [weakSelf.dataSource addObject:data];
        }
    }

    double delayTime = 3.0;
    dispatch_time_t time = dispatch_time(DISPATCH_TIME_NOW, delayTime * NSEC_PER_SEC);
    dispatch_after(time, dispatch_get_main_queue(), ^{
            [weakSelf.refreshTableView reloadData];
            [weakSelf.refreshFooterView stopRefresh];
        });
    }];
}
```

## Gif
![](https://github.com/Yuzeyang/TableRefreshAnimation/raw/master/TableRefreshAnimation1.gif)
![](https://github.com/Yuzeyang/TableRefreshAnimation/raw/master/TableRefreshAnimation2.gif)