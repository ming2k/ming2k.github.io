---
title: why gnome ditches the tray icons
date: 2024-04-24
---

# 为什么Gnome移除托盘图标

参考：

- [AppIndicator support in GNOME 3](https://discourse.gnome.org/t/appindicator-support-in-gnome-3/1807)
- [Status Icons and GNOME](https://blogs.gnome.org/aday/2017/08/31/status-icons-and-gnome/)



首先就是托盘图标这东西在 通知中心、MPRIS（统一控制媒体播放的）、系统搜索、同步服务之前创造出来的产物，比如之前windows没有通知中心，接受消息都是靠着比如QQ图标闪烁和声音提示，比如android出来比较晚，但是由于通知中心的存在，所以就没必要在小小的屏幕中放很多托盘图标，而且可以清晰的看到消息来源和内容，当gnome有通知栏的时候，gnome放弃托盘从而为开发者和用户提供单一明确的入口，避免消息通知多种途径的零乱，比如QQ音乐右击托盘可以控制音乐的播放/暂停和上下切歌，但是MPRIS统一控制了，避免混乱。

其次是想在系统和应用层级上面划分界限，他认为top bar上的信息都是桌面系统级别的状态信息，如果添加app tray用来显示app的状态信息，就会让用户产生混乱，他们想要避免这种情况。

然后他们认为app tray从用户那里夺取了控制权，这也是gnome热衷沙箱的原因，托盘图标没办法控制我是否让对方弹出信息，他是否应该访问我的某些文件夹，而flatpak这种沙箱安全且可控的解决了这个问题。

最主要一点，gnome是想对触摸屏用户友好一点，托盘图标太小且不好点击。

