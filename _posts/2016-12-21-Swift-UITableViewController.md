---
layout: post
title: "Swift-UITableViewController"
description: ""
category: "Swift"
tags: []
---
##### 一、创建一个MySettingViewController: UITableViewController，由于此控制器已经继承了UITableViewDelegate, UITableViewDataSource，所以在此无需再设置了。
```swift
import UIKit
class MySettingViewController: UITableViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
    }
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
 }
```
#####二、在override func viewDidLoad() {}设置属性和注册UITableViewCell
```swift
private let MySettingCellID = "MySettingTableViewCell"
override func viewDidLoad() {
        super.viewDidLoad()
        
        tableView.register(MySettingTableViewCell.self, forCellReuseIdentifier: MySettingCellID)
        tableView.isScrollEnabled=false
        tableView.backgroundColor = UIColor.white
             
        navigationItem.leftBarButtonItem=UIBarButtonItem.init(image: UIImage(named:"backPersonalCenter"), style: .plain, target: self, action: (#selector(backPersonalCenterViewController)))
    }
```

#####三、UITableViewController的数据源方法：UITableViewDataSource
```swift
// MARK: - Table view data source
    override func numberOfSections(in tableView: UITableView) -> Int {
        return 1
    }
    
    override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return 3
    }
    
    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        
        let cell:MySettingTableViewCell = tableView.dequeueReusableCell(withIdentifier: MySettingCellID, for: indexPath) as! MySettingTableViewCell
        if indexPath.row==0 {
            cell.leftLabel.text="清除缓存"
            cell.rightBtn.setTitle("100MB", for: .normal)
            cell.rightBtn.setTitleColor(UIColor.init(red: 102/255.0, green: 102/255.0, blue: 102/255.0, alpha: 1), for: .normal)
            cell.rightBtn.titleLabel?.font = UIFont.systemFont(ofSize: 13)
        } else if indexPath.row==1 {
            cell.leftLabel.text="意见反馈"
            cell.rightBtn.setImage(UIImage(named: "rightArrow"), for: .normal)
        } else {
            cell.leftLabel.text="版本信息"
            cell.rightBtn.setTitle("V2.03", for: .normal)
            cell.rightBtn.setTitleColor(UIColor.init(red: 102/255.0, green: 102/255.0, blue: 102/255.0, alpha: 1), for: .normal)
            cell.rightBtn.titleLabel?.font = UIFont.systemFont(ofSize: 13)
        }
        return cell
    }
```

#####四、UITableViewController的代理方法：UITableViewDelegate
```swift
override func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat {
        return 53
    }
    
    override func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        if indexPath.row==1 {
            self.navigationController?.pushViewController(FeedbackViewController(), animated: true)
        }
    }
```

#####五、UITableViewController的footerView
```swift
override func tableView(_ tableView: UITableView, viewForFooterInSection section: Int) -> UIView? {
        let footerView = UIView()
        let quitBtn=UIButton()
        quitBtn.backgroundColor=UIColor.init(red: 34/255.0, green: 192/255.0, blue: 184/255.0, alpha: 1)
        quitBtn.setTitle("退出登录", for: .normal)
        quitBtn.titleLabel?.font=UIFont.systemFont(ofSize: 20)
        quitBtn.setTitleColor(UIColor.white, for: .normal)
        footerView.addSubview(quitBtn)//可以自定义quitBtn
        
        quitBtn.snp.makeConstraints { (make) in
            make.width.equalTo(340)
            make.height.equalTo(48)
            make.centerX.equalTo(footerView.snp.centerX)
            make.bottom.equalTo(footerView.snp.bottom)
        }
    
        return footerView
    }
```

#####六、自定义UITableViewCell
```swift
//  MySettingTableViewCell.swift
import UIKit
class MySettingTableViewCell: UITableViewCell {
    var leftLabel: UILabel = UILabel(title:"XXXX",fontSize:13,color:UIColor.init(red: 102/250.0, green: 102/250.0, blue: 102/250.0, alpha: 1))
    var rightBtn:UIButton = UIButton()
    var bottomLineView:UIView = UIView()
    
    override init(style: UITableViewCellStyle, reuseIdentifier: String?) {
        super.init(style: style, reuseIdentifier: reuseIdentifier)
        
        setUpUI()
        self.selectionStyle = .none
    }
    
    required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    func setUpUI() {
        self.addSubview(leftLabel)
        self.addSubview(rightBtn)
        self.addSubview(bottomLineView)
        bottomLineView.backgroundColor = UIColor.init(red: 220/255.0, green: 221/255.0, blue: 221/255.0, alpha: 1)
        
        leftLabel.snp.makeConstraints { (make) in
            make.centerY.equalTo(self)
            make.height.equalTo(13)
            make.left.equalTo(self).offset(16)
        }
        
        rightBtn.snp.makeConstraints { (make) in
            make.centerY.equalTo(self)
            make.height.equalTo(17)
            make.right.equalTo(self).offset(-16)
        }
        
        bottomLineView.snp.makeConstraints { (make) in
            make.left.equalTo(self).offset(16)
            make.height.equalTo(1)
            make.right.equalTo(self)
            make.bottom.equalTo(self)
        }       
    }
    
    override func awakeFromNib() {
        super.awakeFromNib()
        // Initialization code
    }
    
    override func setSelected(_ selected: Bool, animated: Bool) {
        super.setSelected(selected, animated: animated)
        
        // Configure the view for the selected state
    }
}

```