# スワイプトランジションフィルタ処理

![Preview coreimage013](./img/coreimage013.png)

## Swift 3.0 
```swift
//
//  ViewController.swift
//  CoreImage013
//
//  Created by Misato Morino on 2016/08/15.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import UIKit
import CoreImage
import Foundation

extension UIImage{
    
    class func ResizeÜIImage(image : UIImage,width : CGFloat, height :
        CGFloat)-> UIImage!{
        
        // 指定された画像の大きさのコンテキストを用意.
        UIGraphicsBeginImageContext(CGSize(width: width, height: height))
        
        // コンテキストに画像を描画.
        image.draw(in: CGRect(x: 0, y: 0, width: width, height: height))
        
        // コンテキストからUIImageを生成.
        let newImage = UIGraphicsGetImageFromCurrentImageContext()
        
        // コンテキストを閉じる.
        UIGraphicsGetImageFromCurrentImageContext()
        
        return newImage
    }
}

class ViewController: UIViewController{
    
    // UIImageViewnの生成.
    var myImageView:UIImageView!
    var count = 0.0
    var myTime = NSNumber(value:0.0)
    let myFilter = CIFilter(name: "CISwipeTransition")
    
    // 画像遷移の時間処理.
    func update(timer: Timer) {
        
        count += 0.15
        myTime = count
        
        myFilter!.setValue(myTime, forKey: kCIInputTimeKey)
        
        // 加工後の画像を得る.
        let myOutputImage = UIImage(ciImage: myFilter!.outputImage!)
        
        //加工後の画像をUIImageViewに設定する.
        myImageView.image = myOutputImage
        
    }
    
    override func viewDidLoad() {
        
        // UIImageViewを作る.
        myImageView = UIImageView(frame: self.view.bounds)
        self.view.addSubview(myImageView)
        
        // 変換元の画像と遷移の終着点の画像を作る.
        let CIImage1 = CIImage(image: UIImage.ResizeÜIImage(image: UIImage(named:"sample1")!,width: self.view.frame.maxX, height:self.view.frame.maxY))
        
        let CIImage2 = CIImage(image: UIImage.ResizeÜIImage(image: UIImage(named: "sample2")!, width: self.view.frame.maxX, height:self.view.frame.maxY))
        
        // フィルタ処理の角度を指定する.
        let myAngle = NSNumber(value: 0.6)
        // フィルタ処理の幅を指定する.
        let myWidth = NSNumber(value:200)
        
        myFilter!.setValue(myWidth,  forKey: kCIInputWidthKey)
        myFilter!.setValue(myAngle,  forKey: kCIInputAngleKey)
        myFilter!.setValue(CIImage1, forKey: kCIInputImageKey)
        myFilter!.setValue(CIImage2, forKey: kCIInputTargetImageKey)
        
        // Nstimerの設定.
        Timer.scheduledTimer(timeInterval: 1, target: self, selector: #selector(ViewController.update(timer:)), userInfo: nil, repeats: true)
        
        // 加工後の画像を得る.
        let myOutputImage = UIImage(ciImage: myFilter!.outputImage!)
        
        // 加工後の画像をUIImageViewに設定.
        myImageView.image = myOutputImage
    }
    
}
```

# Swift 2.3 
```swift
//
//  ViewController.swift
//  CoreImage013
//
//  Created by Misato Morino on 2016/08/15.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import UIKit
import CoreImage
import Foundation

extension UIImage{
    
    class func ResizeÜIImage(image : UIImage,width : CGFloat, height :
        CGFloat)-> UIImage!{
        
        // 指定された画像の大きさのコンテキストを用意.
        UIGraphicsBeginImageContext(CGSizeMake(width, height))
        
        // コンテキストに画像を描画.
        image.drawInRect(CGRectMake(0, 0, width, height))
        
        // コンテキストからUIImageを生成.
        let newImage = UIGraphicsGetImageFromCurrentImageContext()
        
        // コンテキストを閉じる.
        UIGraphicsGetImageFromCurrentImageContext()
        
        return newImage
    }
}

class ViewController: UIViewController{
    
    // UIImageViewnの生成.
    var myImageView:UIImageView!
    var count = 0.0
    var myTime = NSNumber(double:0.0)
    let myFilter = CIFilter(name: "CISwipeTransition")
    
    // 画像遷移の時間処理.
    func update(timer: NSTimer) {
        
        count += 0.15
        myTime = count
        
        myFilter!.setValue(myTime, forKey: kCIInputTimeKey)
        
        // 加工後の画像を得る.
        let myOutputImage = UIImage(CIImage: myFilter!.outputImage!)
        
        //加工後の画像をUIImageViewに設定する.
        myImageView.image = myOutputImage
        
    }
    
    override func viewDidLoad() {
        
        // UIImageViewを作る.
        myImageView = UIImageView(frame: self.view.bounds)
        self.view.addSubview(myImageView)
        
        // 変換元の画像と遷移の終着点の画像を作る.
        let CIImage1 = CIImage(image: UIImage.ResizeÜIImage(UIImage(named:"sample1")!,width: self.view.frame.maxX, height:self.view.frame.maxY))
        
        let CIImage2 = CIImage(image: UIImage.ResizeÜIImage(UIImage(named: "sample2")!, width: self.view.frame.maxX, height:self.view.frame.maxY))
        
        // フィルタ処理の角度を指定する.
        let myAngle = NSNumber(double: 0.6)
        // フィルタ処理の幅を指定する.
        let myWidth = NSNumber(double:200)
        
        myFilter!.setValue(myWidth,  forKey: kCIInputWidthKey)
        myFilter!.setValue(myAngle,  forKey: kCIInputAngleKey)
        myFilter!.setValue(CIImage1, forKey: kCIInputImageKey)
        myFilter!.setValue(CIImage2, forKey: kCIInputTargetImageKey)
        
        // Nstimerの設定.
        NSTimer.scheduledTimerWithTimeInterval(1, target: self, selector: #selector(ViewController.update(_:)), userInfo: nil, repeats: true)
        
        // 加工後の画像を得る.
        let myOutputImage = UIImage(CIImage: myFilter!.outputImage!)
        
        // 加工後の画像をUIImageViewに設定.
        myImageView.image = myOutputImage
    } 
}
```

## 2.xと3.xの差分
* ```init(CIImage:)``` から ```init(ciImage:)``` に変更
* ```scheduledTimerWithTimeInterval``` から ```scheduledTimer``` に変更

## Reference

* CIFilter
    * [https://developer.apple.com/reference/coreimage/cifilter](https://developer.apple.com/reference/coreimage/cifilter)
* CIImage
    * [https://developer.apple.com/reference/coreimage/ciimage](https://developer.apple.com/reference/coreimage/ciimage)
