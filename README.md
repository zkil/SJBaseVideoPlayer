![readme](https://user-images.githubusercontent.com/37614260/43947531-922a0712-9cb2-11e8-8f8d-4823a21308d3.png)

[![Build Status](https://travis-ci.org/changsanjiang/SJVideoPlayer.svg?branch=master)](https://travis-ci.org/changsanjiang/SJVideoPlayer)
[![Version](https://img.shields.io/cocoapods/v/SJVideoPlayer.svg?style=flat)](https://cocoapods.org/pods/SJVideoPlayer)
[![Platform](https://img.shields.io/badge/platform-iOS-blue.svg)](https://github.com/changsanjiang)
[![License](https://img.shields.io/github/license/changsanjiang/SJVideoPlayer.svg)](https://github.com/changsanjiang/SJVideoPlayer/blob/master/LICENSE.md)

### Installation
```ruby
# Player with default control layer.
pod 'SJVideoPlayer'

# The base player, without the control layer, can be used if you need a custom control layer.
pod 'SJBaseVideoPlayer'

# 天朝
# 如果网络不行安装不了, 可改成以下方式进行安装
pod 'SJBaseVideoPlayer', :git => 'https://gitee.com/changsanjiang/SJBaseVideoPlayer.git'
pod 'SJVideoPlayer', :git => 'https://gitee.com/changsanjiang/SJVideoPlayer.git'
pod 'SJUIKit/AttributesFactory', :git => 'https://gitee.com/changsanjiang/SJUIKit.git'
pod 'SJUIKit/ObserverHelper', :git => 'https://gitee.com/changsanjiang/SJUIKit.git'
pod 'SJUIKit/Queues', :git => 'https://gitee.com/changsanjiang/SJUIKit.git'
$ pod update --no-repo-update   (不要用 pod install 了, 用这个命令安装)
```
- [Base Video Player](https://github.com/changsanjiang/SJBaseVideoPlayer)

___

## Contact
* Email: changsanjiang@gmail.com

___

## License
SJVideoPlayer is available under the MIT license. See the LICENSE file for more info.

___

## Documents

#### [1. 视图层次结构](#1)

<p>
为防止 Cell 复用, 通过指定视图层次结构, 使得播放器能够定位具体的父视图, 依此来控制隐藏与显示.  以下为目前支持的视图层次:
</p>

* [1.1 UIView](#1.1)
* [1.2 UITableView](#1.2)
    * [1.2.1 UITableViewCell](#1.2.1)
    * [1.2.2 UITableViewHeaderView](#1.2.2)
    * [1.2.3 UITableViewFooterView](#1.2.3)
    * [1.2.4 UITableViewHeaderFooterView](#1.2.4)
* [1.3 UICollectionView](#1.3)
    * [1.3.1 UICollectionViewCell](#1.3.1)
* [1.4  嵌套时的视图层次](#1.4)
    * [1.4.1 UICollectionView 嵌套在 UITableViewCell 中](#1.4.1)
    * [1.4.2 UICollectionView 嵌套在 UITableViewHeaderView 中](#1.4.2)
    * [1.4.3 UICollectionView 嵌套在 UICollectionViewCell 中](#1.4.3)

___

<h3 id="1.1">1.1 UIView</h3>  

<p>
在普通视图中播放时, 不需要指定视图层次, 直接创建资源进行播放即可. 代码如下: 
</p>

```Objective-C
_player = [SJVideoPlayer player];
_player.view.frame = ...;
[self.view addSubview:_player.view];

// 设置资源进行播放
SJVideoPlayerURLAsset *asset = [[SJVideoPlayerURLAsset alloc] initWithURL:URL];
_player.URLAsset = asset;
```

<h3 id="1.2">1.2 UITableView</h3>

<p>
在 UITableView 中播放时, 需指定视图层次, 使得播放器能够定位具体的父视图, 依此来控制隐藏与显示.
</p>

<h3 id="1.2.1">1.2.1 UITableViewCell</h3>

<p>
在 UITableViewCell 中播放时, 需指定 Cell 所处的 indexPath 以及播放器父视图的 tag. 

在滑动时, 管理类将会通过这两个参数控制播放器父视图的显示与隐藏.
</p>

```Objective-C
--  UITableView
    --  UITableViewCell
        --  Player.superview
            --  Player.view


_player = [SJVideoPlayer player];

UIView *playerSuperview = cell.coverImageView;
SJPlayModel *playModel = [SJPlayModel UITableViewCellPlayModelWithPlayerSuperviewTag:playerSuperview.tag atIndexPath:indexPath tableView:self.tableView];

_player.URLAsset = [[SJVideoPlayerURLAsset alloc] initWithURL:URL playModel:playModel];
```
