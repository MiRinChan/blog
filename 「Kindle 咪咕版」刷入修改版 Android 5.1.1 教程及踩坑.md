# 「Kindle 咪咕版」刷入修改版 Android 5.1.1 教程及心得

郑重提示：**本教程涉及对设备有破坏性的行为。越狱有风险，搞机须谨慎。**

任何人不会对你的设备损坏负责。

未成年 Kindle，请在成年 Kindle 的监护下观看。
> 不要对刷好后的 Kindle 抱有太多的期望，系统有问题，很多组件都没有！
> 
> 以下应用经测试，用不了。
> 
> Bromite / Chrome / QQ / 微信 / 微信键盘 / Iceraven / Firefox（不能用 OpenGL，不能用带内核的浏览器）、Matsuri (没有 VPN 组件）、各种下载器（DownloadProvider 砍掉了）、你换不了 webview（亚马逊貌似整了个自己的 webview）
> 
> Magisk 不能刷模块。
> 
> 不支持 scrcpy。

<img width="2880" height="1620" alt="由 CrackDroid Project 支持" src="https://github.com/user-attachments/assets/cd4884fc-e80f-4e42-9552-e4e875b4fc64" />

## 准备材料
 - 正常运作的 Windows 10 电脑
 - 要有一个正常的鼠标和键盘
 - 还有手，不会手贱的那种
 - TTL 线与连接 TTL 的设备（淘宝关键词：USB TO TTL）
 - 焊棒（你要能飞线）/ 让 TTL 线粘住的胶布（别把电容干下来了）/ TTL 夹子（淘宝关键词：TTL 夹子）

## 准备软件
 - PuTTY（或同类软件）
 - 如果你购买了像 USB TO TTL 这样的小玩意的话呢，你可能需要安装一个驱动。
 - ADB
 - 其他驱动

## 第零步 准备

你需要先把你的 Kindle 充好电，不要给你刷一半忽然关机了。

PCB 板普遍熔点为 200℃ 左右，如果你要用电焊的话，先练好再说。

下载懒人包：https://pan.baidu.com/s/19lW__xV49NHfcpjTBUZpXQ?pwd=migu

或者前往 QQ 群获取最新的下载包：758 09 7314

装好 ADB 驱动就可以开始了。

## 第一步 拆机

1. 首先从充电口的位置使用拆机片撬开，有卡扣，需要一个个拆下来。

<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/2fc3fb65-f586-44ec-a247-041515e8febe" />

2. 然后拆屏幕边框，这个没有卡扣，但是绕着屏幕一圈有胶。拆下来的时候需要注意四角不要搞断了，拆下这个边框后，就不能触摸了，只有装上这个才可以触摸。建议从左下角开始拆 _（你也可以大力出奇迹，直接从左下角撕下来（误））_

<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/cf7ff5a5-9141-475c-8b17-00178c7874e6" />

3. 拆下 7 颗螺丝，主板的底部松一松向上推一下即可把主板拆下来。

## 第二步 刷机

1. 将 TTL 连接到 Kindle 上。三条线必须要接上，GND 可以接屏蔽罩上。如果你用的 TTL 夹子的话可以直接接上去。

<img width="495" height="683" alt="TTL 接线图" src="https://github.com/user-attachments/assets/2e337a5d-25a5-4192-84ac-c70c04ec6a5a" />

2. 打开 PuTTY 应用，配置中选择 Serial（串口），Serial line 可以到设备管理器中查看。Speed（波特率）调 115200。配置完毕后点击 Open 打开会话。

<img width="602" height="543" alt="PuTTY 配置图" src="https://github.com/user-attachments/assets/717fefe1-ac0c-4e6d-abf8-f7779aff2f35" />

3. 接好 TTL 后点击一下电源键看看是否有反应，这里要注意一下，一些 USB TO TTL 的定义不一样，可能 TXD 和 RXD 是相反的，所以如果没有反应的话把上面两个针脚反过来接试试。
4. 测试无误后可以长按电源键强制重启，留意 PuTTY 的窗口，疯狂按回车键直到输出 hit any key to autostop，uboot>。这就打断了开机流程，依次输入以下内容。

```sh
setenv bootcmd 'mmc dev ${mmcdev};if mmc rescan; then fuse override 4 6 0x00000020; run testboot; fi;'

saveenv

fuse override 4 6 0x00000020

fastboot
```

5. 这时候机器应该重启到了 Fastboot 模式，这时候打开下载和镜像的压缩包，_解压_下来 （不要直接压缩软件里在里面运行），解压完成后打开 `fastdownload.bat`，按照脚本里的操作就好了。

（如果打开没有反应的话就是驱动没弄好，打开设备管理器，找到未知设备，右击-更新驱动程序，点击`浏览我的电脑以查找驱动程序`，点击`让我从计算机的可用驱动程序列表中选取`，找到 `Android Device`，选择 `Android Bootloader Interface`，如有多个，选第一个。然后再试。）

（进入 TWRP 的话要把那个滑块给划掉，可以套上屏幕边框操作）

（如果你不清楚你的空间够不够，进 TWRP 后先别操作电脑，进入右上角的 `Wipe`，点击 `Format Data`，输入 `yes` 来清除，此时点击左边的 Back，再点击返回键，回到主页后点击 `Reboot`，点击 `Recovery`，点击 `Do Not Install`，等一下后在电脑里的脚本选 `2`，之后按脚本操作）

## 第三步 完成

完成啦，装回去就好了，注意，你有七颗螺丝要装。

### 注意事项
1. 不要直接安装升级 Magisk（除非你会其他的方式安装）
2. 不要重置系统。

如果一不小心重置了，拆机接 TTL，然后重启疯狂按回车打断开机流程。

输入 `fuse override 4 6 0x00000020`
然后输入 `fastboot`

进入到 fastboot 后重新重复本教程第二步第⑤项即可。

3. 不要再修改 U-Boot，不然你会永远卡在下载模式！

<img width="1280" height="960" alt="完成图" src="https://github.com/user-attachments/assets/8212faaa-067d-4648-9067-f7b13313d97c" />


### 好玩的
可以换掉背景图，修改 `/system/priv-app/SystemUI/SystemUI.apk/res/drawable-mdpi-v4/bg_ss**.png` 的图片。

图片格式需要 `png`，分辨率为 `600 × 800`。文件格式命名为 `bg_ss**.png`，打开 7-Zip 直接替换就好了。但是必须要放满，从 `bg_ss00.png` 到 `bg_ss13.png`，**一张也不能少！！**

完事后不要签名直接替换回去。**签名会导致开不了机！！！！！**

## Credit
[Ygjsz_](https://space.bilibili.com/33072537)


<img width="320" alt="image" src="https://github.com/user-attachments/assets/94a9e4a7-063c-4f92-80e5-be766fc9a901" />
<img width="320" alt="image" src="https://github.com/user-attachments/assets/e0b8723c-07a5-423b-8e38-8b98987a789b" />

