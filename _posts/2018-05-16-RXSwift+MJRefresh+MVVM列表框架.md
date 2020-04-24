---
title: RXSwift+MJRefresh+MVVM列表框架
date: 2018-05-16
tags: 框架
categories: Swift
---



## 快速实现列表上下拉刷新

### 1. 创建viewModel类

该类继承MGRefreshable，重载requestMethod获取列表接口数据，并传递给rx的订阅者

<!-- more -->

```swift
class MGProjectViewModel: MGRefreshable {
    public var param: [String: Any]?
    
    override func requestMethod(_ observer: (AnyObserver<Any>)) {
        MGRequest.listData(MGProjectModel.self, API: MGProjectAPI.self, target: .projectList(page: currentPage, size: pageSize, param: param), success: { (data) in
            observer.onNext(data)
            observer.onCompleted()
        }, failure: { (_, _) in
            observer.onCompleted()
        })
    }
}
```



### 2. viewController初始化viewModel并关联tableview

startRequest和下拉刷新的逻辑一样，但不会触发下拉的UI效果



```swift
let viewModel = MGProjectViewModel()

override func viewDidLoad() {
    super.viewDidLoad()
    // Do any additional setup after loading the view.
    ...
    viewModel.configListRefresh(tableView: tableView)
    viewModel.startRequest()
}
```



tableview的cell逻辑

```swift
extension MGProjectViewController: UITableViewDelegate, UITableViewDataSource {
    
    //设置cell的数量
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return viewModel.dataSource.count
    }
    
    //设置tableview的cell
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: MGDefaultCellID, for: indexPath) as! MGDefaultTableViewCell
        let model = viewModel.dataSource[indexPath.row] as! MGProjectModel
        cell.title = model.name
        return cell
    }
}
```





## 组件封装源码

MGRefreshable.swift封装上下拉刷新和监听列表数据逻辑



```swift
import UIKit
import RxSwift

protocol MGRefreshableData {
    func requestMethod(_ observer: (RxSwift.AnyObserver<Any>))
    
}

class MGRefreshable: MGRefreshableData {
    func requestMethod(_ observer: (AnyObserver<Any>)) {
        
    }
    
    var total: Int = 0
    var currentPage: Int = 1
    var pageSize: Int = 20
    var isNoMore: Bool = false
    var hiddenMJHeader: Bool = false
    var hiddenMJFooter: Bool = false
    var dataSource = Array<Any>()//列表数据源
    private var refreshData = Array<Any>() {
        //当前页接口数据
        willSet(data) {
            if (currentPage == 1) {
                self.dataSource.removeAll()
            }
            isNoMore = data.count < self.pageSize
            self.dataSource = self.dataSource + data
        }
    }
    
    private var scrollView: UIScrollView?
    private let disposeBag = DisposeBag()
    
    lazy var refreshObservable = Observable.create { (observer:AnyObserver<Any>) -> Disposable in
        self.requestMethod(observer)
        return Disposables.create()
    }
    
}

extension MGRefreshable {
    func startRequest() {
        let tableView = scrollView as! UITableView
        currentPage = 1
        refreshObservable.subscribe(onNext: { (obj) in
            self.refreshData = obj as! [Any]
        }, onCompleted: {
            tableView.reloadData()
            if self.dataSource.count > 0 {
                tableView.setContentOffset(CGPoint.zero, animated: true)
            }
            if self.isNoMore {
                tableView.mj_footer.endRefreshingWithNoMoreData()
            }
        }).disposed(by: self.disposeBag)
    }
    
    func configListRefresh(tableView: UITableView) {
        scrollView = tableView
        //下拉控件
        if !hiddenMJHeader {
            let header = MJRefreshNormalHeader.init(refreshingBlock: {
                tableView.mj_footer.state = .idle
                self.currentPage = 1
                self.refreshObservable.subscribe(onNext: { (obj) in
                    self.refreshData = obj as! [Any]
                }, onCompleted: {
                    tableView.reloadData()
                    tableView.mj_header.endRefreshing()
                    if self.isNoMore {
                        tableView.mj_footer.endRefreshingWithNoMoreData()
                    }
                }).disposed(by: self.disposeBag)
            })
            header?.lastUpdatedTimeLabel.isHidden = true
            header?.stateLabel.isHidden = true
            tableView.mj_header = header
        }
        
        //上拉控件
        if self.hiddenMJFooter { return }
        tableView.mj_footer = MJRefreshBackNormalFooter.init(refreshingBlock: {
            tableView.mj_header.state = .idle
            self.currentPage += 1
            self.refreshObservable.subscribe(onNext: { (obj) in
                self.refreshData = obj as! [Any]
            }, onCompleted: {
                tableView.reloadData()
                tableView.mj_header.endRefreshing()
                if self.isNoMore {
                    tableView.mj_footer.endRefreshingWithNoMoreData()
                } else {
                    tableView.mj_footer.endRefreshing()
                }
            }).disposed(by: self.disposeBag)
        })
    }
}
```