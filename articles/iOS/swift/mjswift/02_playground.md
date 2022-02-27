# Playground

## view

<img src="https://gitee.com/dexport/blog-image/raw/master/img/202202280014114.png" style="zoom:50%;" />

```swift
import UIKit
import PlaygroundSupport

let view = UIView()
view.frame = CGRect(x: 0, y: 0, width: 100, height: 100)
view.backgroundColor = UIColor.red
PlaygroundPage.current.liveView = view

let imageView = UIImageView(image: UIImage(named: "banana"))
PlaygroundPage.current.liveView = imageView

let vc = UITableViewController()
vc.view.backgroundColor = UIColor.lightGray
PlaygroundPage.current.liveView = vc
```

## 多page

<img src="https://gitee.com/dexport/blog-image/raw/master/img/202202280022094.png" style="zoom:50%;" />

