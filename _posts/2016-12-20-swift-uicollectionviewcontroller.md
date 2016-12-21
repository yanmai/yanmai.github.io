---
layout: post
title: "Swift之UICollectionViewController"
description: ""
category: "Swift"
tags: []
---

`UICollectionViewController` 和 `UITableViewController` 这两个控制器，经常会在开发中被使用到。这次先主要展示一下用 Swift 怎样写 `UICollectionViewController`，具体代码如下：

##### 一、创建一个UICollectionViewController：`storeCollectionViewController.swift` ，由于此控制器已经继承了 `UICollectionViewDelegate, UICollectionViewDataSource`，所以在此设置一个 `UICollectionViewDelegateFlowLayout` 的代理就可以了。

```swift
import UIKit
class storeCollectionViewController: UICollectionViewController,UICollectionViewDelegateFlowLayout {
    override func viewDidLoad() {
        super.viewDidLoad()
    }
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }
}
```

##### 二、在 `override func viewDidLoad() {}` 设置属性和注册需要的自定义 `headerView` 和 `UICollectionViewCell`，为便于大家理解，相关自定义的代码会在贴在此文的最后

```swift
override func viewDidLoad() {
    super.viewDidLoad()

    //注册自定义headerView
    collectionView?.register(picturesCarouselHeaderView.self, forSupplementaryViewOfKind: UICollectionElementKindSectionHeader, withReuseIdentifier: picturesCarouselHeaderViewID)

    //注册自定义cell(纯代码)
    collectionView?.register(firstSectionCollectionViewCell.self, forCellWithReuseIdentifier: firstSectionCellID)

    //注册自定义cell(Xib)
    let cellNib = UINib(nibName: "goodsCollectionViewCell", bundle: nil)
    collectionView?.register(cellNib, forCellWithReuseIdentifier: reCellID)

    collectionView?.backgroundColor = UIColor.init(red: 239/255.0, green: 239/255.0, blue: 239/255.0, alpha: 1)

    //设置collectionView顶到状态栏的顶部
    self.automaticallyAdjustsScrollViewInsets = false
}
```

##### 三、UICollectionViewController的数据源方法：`UICollectionViewDataSource`

```swift
// MARK: UICollectionViewDataSource
override func numberOfSections(in collectionView: UICollectionView) -> Int {
    return 2
}
override func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
    if section == 0 {
        return 1
    }
    return 20
}
override func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {

    if indexPath.section == 0 {

        let cell:firstSectionCollectionViewCell = collectionView.dequeueReusableCell(withReuseIdentifier: firstSectionCellID, for: indexPath) as! firstSectionCollectionViewCell
        cell.backgroundColor=UIColor.white
        return cell
    }

    let cell:goodsCollectionViewCell = collectionView.dequeueReusableCell(withReuseIdentifier: reCellID, for: indexPath) as! goodsCollectionViewCell

    cell.backgroundColor=UIColor.white

    cell.goodStatusImageView.image=UIImage(named: "s_XXXX")
    cell.goodImageView.image=UIImage(named: "icon_XXXX")
    cell.goodNameLabel.text="XXXX"
    cell.goodPriceLabel.text="XXXX"

    return cell
}
```

##### 四、UICollectionViewController的代理方法：`UICollectionViewDelegate`

```swift
//MARK: UICollectionViewDelegate
override func collectionView(_ collectionView: UICollectionView, shouldSelectItemAt indexPath: IndexPath) -> Bool {

    if indexPath.section==0 {
        return false
    }
    return true
}
override func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {

    let goodsDetailViewController = GoodsDetailViewController.init()
    self.navigationController?.pushViewController(goodsDetailViewController, animated: true)
}
```

##### 五、UICollectionViewController布局的相关代码：`UICollectionViewDelegateFlowLayout`

```swift
init(){    
    let layout:UICollectionViewFlowLayout = UICollectionViewFlowLayout()
    // 设置四边间距
    layout.minimumLineSpacing = 1;
    layout.minimumInteritemSpacing = 0;

    super.init(collectionViewLayout: layout)
}

required init?(coder aDecoder: NSCoder) {
    fatalError("init(coder:) has not been implemented")
}

```

```swift
//MARK: -UICollectionViewDelegateFlowLayout
func  collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {

    if indexPath.section == 0 {
        return CGSize(width: UIScreen.main.bounds.width, height: 68)
    }
    return CGSize(width: (UIScreen.main.bounds.width-0.5)/2, height: 250)

}

//返回section上下左右的间距
func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, insetForSectionAt section: Int) -> UIEdgeInsets {
    if section==0 {
        return UIEdgeInsetsMake(0, 0, 0, 0)
    }
    return UIEdgeInsetsMake(1, 0, 1, 0)
}
```

##### 六、UICollectionViewController的自定义`headerView`

```swift
//创建headerView
override func collectionView(_ collectionView: UICollectionView, viewForSupplementaryElementOfKind kind: String, at indexPath: IndexPath) -> UICollectionReusableView {

    var headerView = picturesCarouselHeaderView()

    if kind==UICollectionElementKindSectionHeader {

        headerView=collectionView.dequeueReusableSupplementaryView(ofKind: kind, withReuseIdentifier: picturesCarouselHeaderViewID, for: indexPath) as! picturesCarouselHeaderView
    }

    headerView.backgroundColor=UIColor.init(red: 227/255.0, green: 227/255.0, blue: 227/255.0, alpha: 1)

    return headerView
}

//返回headerView的宽高
func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, referenceSizeForHeaderInSection section: Int) -> CGSize {

    if section==0 {
        return CGSize(width: UIScreen.main.bounds.width, height: 234)
    }
    return CGSize(width: 0, height: 0)
}
```

##### 七、自定义 `HeaderView` 和 `UICollectionViewCell` 的相关代码

1、纯代码自定义UICollectionViewCell: `firstSectionCollectionViewCell.swift`

```swift
import UIKit
class firstSectionCollectionViewCell: UICollectionViewCell {

    lazy var lineView : UIView = {
      let view = UIView()
        view.backgroundColor=UIColor.init(red: 239/255.0, green: 239/255.0, blue: 239/255.0, alpha: 1)
      return view
    }()

    lazy var titleLabel: UILabel = UILabel(title:"全部商品",fontSize:17,color:UIColor.black)    
    lazy var detailedTitleLabel: UILabel = UILabel(title:"好东西都在这哦",fontSize:13,color:UIColor.black)

    override init(frame: CGRect) {
        super.init(frame: frame)

        setUpUI()
    }

    func setUpUI() {
        self.addSubview(lineView)
        self.addSubview(titleLabel)
        self.addSubview(detailedTitleLabel)

        lineView.snp.makeConstraints { (make) in
            make.left.equalTo(self.snp.left)
            make.right.equalTo(self.snp.right)
            make.top.equalTo(self.snp.top)
            make.height.equalTo(10)
        }

        titleLabel.snp.makeConstraints { (make) in
            make.left.equalTo(self.snp.left)
            make.right.equalTo(self.snp.right)
            make.top.equalTo(lineView.snp.bottom).offset(12)
            make.height.equalTo(17)
        }

        detailedTitleLabel.snp.makeConstraints { (make) in
            make.left.equalTo(self.snp.left)
            make.right.equalTo(self.snp.right)
            make.top.equalTo(titleLabel.snp.bottom).offset(7)
            make.height.equalTo(13)
        }

    }
    required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
}
```


2、Xib自定义UICollectionViewCell的代码部分(使用xib创建控件部分略)：`firstSectionCollectionViewCell.swift`

 ```swift
 import UIKit
class goodsCollectionViewCell: UICollectionViewCell {

    @IBOutlet weak var goodStatusImageView: UIImageView!
    @IBOutlet weak var goodImageView: UIImageView!
    @IBOutlet weak var goodNameLabel: UILabel!
    @IBOutlet weak var goodPriceLabel: UILabel!

    override func awakeFromNib() {
        super.awakeFromNib()
    }   
}
 ```

3、自定UICollectionView的headerView：重点是我们的headerView要继承`UICollectionReusableView`

```swift
import UIKit
class picturesCarouselHeaderView: UICollectionReusableView {

}
```

一般会在headerView来做我们的图片轮播，后期我会用swift来实现。

##### 八、在很多项目中我们也会用到自定义的`UICollectionView`

```swift
//  StoreViewController.swift
import UIKit
class StoreViewController: UIViewController,UICollectionViewDataSource,UICollectionViewDelegate,UICollectionViewDelegateFlowLayout {

    //goodsCollectionView
    fileprivate lazy var goodsCollectionView: UICollectionView = {

        var layout:UICollectionViewFlowLayout = UICollectionViewFlowLayout()
        // 设置cell四边间距
        layout.minimumLineSpacing = 1;
        layout.minimumInteritemSpacing = 0;

        let collectionView=UICollectionView(frame: CGRect.init(x: 0, y: 0, width: UIScreen.main.bounds.width, height: UIScreen.main.bounds.height), collectionViewLayout: layout)
        //注册自定义headerView
        collectionView.register(PicturesCarouselHeaderView.self, forSupplementaryViewOfKind: UICollectionElementKindSectionHeader, withReuseIdentifier: picturesCarouselHeaderViewID)

        //注册自定义cell(纯代码)
        collectionView.register(FirstSectionCollectionViewCell.self, forCellWithReuseIdentifier: firstSectionCellID)

        //注册自定义cell(Xib)
        let cellNib = UINib(nibName: "GoodsCollectionViewCell", bundle: nil)
        collectionView.register(cellNib, forCellWithReuseIdentifier: reCellID)

        collectionView.backgroundColor = UIColor.init(red: 239/255.0, green: 239/255.0, blue: 239/255.0, alpha: 1)

        //设置collectionView顶到状态栏的顶部
        self.automaticallyAdjustsScrollViewInsets = false

        collectionView.delegate = self
        collectionView.dataSource = self

        return collectionView
    }()
    override func viewDidLoad() {
        super.viewDidLoad()

        view.addSubview(goodsCollectionView)

    }
}
```

Swift作为苹果公司力推的iOS编程的新宠儿，一些新的项目目前已经使用了Swift。为了不让自己被这股浪潮无情的淘汰，所以欣然地接受他；Swift语言也在不断更新，希望在这里我们可以共同进步。
