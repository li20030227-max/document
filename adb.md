```textile
基本文件路径
usb HDMI AV渠道：system/app/qcode.projector.launcher.4380

ui界面

代码：
Y:\code\vendor\mediatek\proprietary_tv\open\custom\bolai\BL_P3NLauncher\app\src\main\java

资源文件位置、layout、图标等：
Y:\code\vendor\mediatek\proprietary_tv\open\custom\bolai\BL_P3NLauncher\app\src\main\res

android:id="@+id/main_hdmi" 

android:id="@+id/main_hdmi2"

android:id="@+id/main_av"

android:id="@+id/main_usb"
在次布局文件里，你搜这几个对应的是主页的那几个信号源（usb、hdmi）。
要隐藏或者改大小，你看对应的属性。其他地方定位也可以按这个参考这个来定位，
Android建议把那几个布局了解下，这样后面找位置你会心里有个概念。

如果不知道是哪个activity的时候可以用串口或者无线连接adb
使用dumpsys activity activities，会打印出对应的activity
（dumpsys activity top显示的内容更多，后面可以进一步了解）。
对应的apk可以用 pm path 包名 查看。

------------------------------------
查看所有activity：
dumpsys activity all | grep ACTIVITY

查看当前的activity是什么：
dumpsys activity top | grep ACTIVITY
查看当前窗口的apk是什么：（可以用来查看设置界面）
dumpsys window | grep mCurrentFocus
如：mCurrentFocus=Window{e4e0b37 u0 com.siviton.systemui}  //systemui调用了JoyarGeneralSettings中的包，先编译JoyarGeneralSettings，再编译systemui。

获取当前apk的路径：
pm path +包名
pm path com.eternal.projection.settings            

apk路径
V31:/system/app # ls

开机动画路径
V31:/system/media # ls

创建新文件（用来装apk）
V31:/system/app # mkdir BL_P3NLauncher

修改文件权限 755
V31:/system/app # chmod 755 BL_P3NLauncher

替换apk
adb connect 192.168.98.142 (第一个cmd)
adb root （第二个cmd）
adb remount （第二个cmd）
adb shell （第二个cmd）
adb push Y:\code\out\target\product\mt5862_bga_1g\system\app\BL_P3NLauncher\BL_P3NLauncher.apk /system/app/BL_P3NLauncher

重启设备
adb reboot

-----------------------------------------
查看当前apk包名和类名
dumpsys window windows | grep -E 'mCurrentFocus|mFocusedApp'

通过aapt查看apk包名和类名（apk直接拖拽进来）
aapt dump badging Y:\code\customer\preinstall_apps\shijiu\HappyCast\HappyCast.apk > E:/apk/info.txt

这个命令会输出 APK 文件的 （versionName），这是用户看到的版本号。
aapt dump badging NetrangeBrowserNrcertArm.apk | grep versionName

如果你想查看内部版本号（versionCode），可以使用以下命令：
aapt dump badging NetrangeBrowserNrcertArm.apk | grep versionCode

通过adb查看apk包的信息
dumpsys package <package_name>
dumpsys package com.qcode.projector.launcher

查看链接路径是否能找到
am start com.joyar.settingslib/.setup.projection.ScreenEntryActivity

启动某个链接
am start .../...
-----------------------------------------

按键路径
/vendor/tvconfig/config/ir_config.ini
/vendor/config/ir_config.ini

查看报错日志
logcat -c;logcat
加上筛选
logcat | grep TAG的名字
如：logcat | grep AboutSettingActivity
强制只查看输入的字符的打印
logcat -s TAG的名字

查看当前所选属性
getprop persist.svt.poweron_source
如：HOME   AV   HDMI 

获取属性
getprop persist.svt.screensaver.loop.show.time
设置属性
setprop persist.svt.screensaver.loop.show.time 20

替换apk
adb connect 192.168.98.142 (第一个cmd)
adb root （第二个cmd）
adb remount （第二个cmd）
adb shell （第二个cmd）
adb push Y:\code\out\target\product\mt5862_bga_1g\system\app\BL_P3NLauncher\BL_P3NLauncher.apk /system/app/BL_P3NLauncher

退出adb shell
exit

重启设备
adb reboot

安装apk
adb install -f XXX.apk
adb -s ip:端口号 install X\X\XXX.apk
adb -s 172.16.16.104:5555 install D:\Work\优酷IoT版【支持遥控器】.apk


（查看安装的apk）（应用）
启动包名为com.android.tv.settings的应用程序中名为MainSettings的Activity。
am start -n com.android.tv.settings/.MainSettings
打开所有应用的应用管理
am start -n com.android.tv.settings/.device.apps.AllAppsActivity

查看蓝牙设备
dumpsys input
如：7: 语音助手
      Classes: 0x80002121
      Path: /dev/input/event5
      Enabled: true
      Descriptor: f668de395e3db2879c36ce0becdb28eb38c9e73a
      Location:
      ControllerNumber: 0
      UniqueId: be:02:61:6a:bf:3b
      Identifier: bus=0x0005, vendor=0x568a, product=0x9869, version=0x0101
      KeyLayoutFile: /vendor/usr/keylayout/Vendor_568a_Product_9869.kl
      KeyCharacterMapFile: /system/usr/keychars/Generic.kcm
      ConfigurationFile:
      HaveKeyboardLayoutOverlay: false

kl蓝牙文件路径
device\mediatek\mt5862_bga_1g\customer\BOLAI_JY9269E_BL42_Z4451080_1920X1080_CN_01C08_V31_BLyinglehui_ylh_20240704\keylayout

进入查看蓝牙按键kl文件中配置是否正确
getevent | grep event5
如：按下按键后出现  c2从16进制变成10进制 然后与kl文件对应
/dev/input/event5: 0004 0004 00070073
/dev/input/event5: 0001 00c2 00000001
/dev/input/event5: 0000 0000 00000000
/dev/input/event5: 0004 0004 00070073
/dev/input/event5: 0001 00c2 00000000
/dev/input/event5: 0000 0000 00000000


Z4:/ # getevent -lt /dev/input/event5
[     627.121607] EV_MSC       MSC_SCAN             0007003e
[     627.121607] EV_KEY       KEY_F5               DOWN
[     627.121607] EV_SYN       SYN_REPORT           00000000

[     629.637845] EV_MSC       MSC_SCAN             0007003e
[     629.637845] EV_KEY       KEY_F5               UP
[     629.637845] EV_SYN       SYN_REPORT           00000000

[     638.094248] EV_MSC       MSC_SCAN             0007004f
[     638.094248] EV_KEY       KEY_RIGHT            DOWN
[     638.094248] EV_SYN       SYN_REPORT           00000000
[     638.286714] EV_MSC       MSC_SCAN             0007004f
[     638.286714] EV_KEY       KEY_RIGHT            UP
[     638.286714] EV_SYN       SYN_REPORT           00000000

[     644.185561] EV_MSC       MSC_SCAN             00070050
[     644.185561] EV_KEY       KEY_LEFT             DOWN
[     644.185561] EV_SYN       SYN_REPORT           00000000
[     644.501776] EV_MSC       MSC_SCAN             00070050
[     644.501776] EV_KEY       KEY_LEFT             UP
[     644.501776] EV_SYN       SYN_REPORT           00000000



遥控配置只有03 和07 的
07的要看

/dev/input/event8: 0004 0004 00070052
/dev/input/event8: 0001 0067 00000001
/dev/input/event8: 0000 0000 00000000
/dev/input/event8: 0004 0004 00070052
/dev/input/event8: 0001 0067 00000000
/dev/input/event8: 0000 0000 00000000


这里看52  就是第6行 第3个数 --- 103
39  static const unsigned char hid_keyboard[256] = {
  	  0,  0,  0,  0, 30, 48, 46, 32, 18, 33, 34, 35, 23, 36, 37, 38,
  	 50, 49, 24, 25, 16, 19, 31, 20, 22, 47, 17, 45, 21, 44,  2,  3,
  	  4,  5,  6,  7,  8,  9, 10, 11, 28,  1, 14, 15, 57, 12, 13, 26,
  	 27, 43, 43, 39, 40, 41, 51, 52, 53, 58, 59, 60, 61, 62, 63, 64,
  	 65, 66, 67, 68, 87, 88, 99, 70,119,110,102,104,111,107,109,106,
  	105,108,103, 69, 98, 55, 74, 78, 96, 79, 80, 81, 75, 76, 77, 71,
  	 72, 73, 82, 83, 86,127,116,117,183,184,185,186,187,188,189,190,
  	191,192,193,194,134,138,130,132,128,129,131,137,133,135,136,113,
  	115,114,unk,unk,unk,121,unk, 89, 93,124, 92, 94, 95,unk,unk,unk,
  	122,123, 90, 91, 85,unk,unk,unk,unk,unk,unk,unk,111,unk,unk,unk,
  	unk,unk,unk,unk,unk,unk,unk,unk,unk,unk,unk,116,unk,unk,unk,unk,
  	unk,unk,unk,unk,unk,unk,179,180,unk,unk,unk,unk,unk,unk,unk,unk,
  	unk,unk,unk,unk,unk,unk,unk,unk,unk,unk,unk,unk,unk,unk,unk,unk,
  	unk,unk,unk,unk,unk,unk,unk,unk,111,unk,unk,unk,unk,unk,unk,unk,
  	 29, 42, 56,125, 97, 54,100,126,164,166,165,163,161,115,114,113,
  	150,158,159,128,136,177,178,176,142,152,173,140,unk,unk,unk,unk

还可以用
logcat | grep keyCode
来看具体的值和当前的按键 红外和蓝牙都可以看
keyCode=KEYCODE_UNKNOWN 就是未知按键
--------------------------
对安卓设备界面截图
--------------------------
先关掉梯形校正（shell）
setprop vendor.mstar.trapezoid.enable 0
再进行截图（shell）
screencap -p 路径/图片名称
screencap -p data/abcde1.jpg
或者（本地）
adb shell screencap -p data/abcde1.jpg
最后导入到本地电脑（本地）
adb pull 路径/图片名称 电脑路径/文件夹
adb pull /data/abcde1.jpg F:/data
删除设备中图片（本wm density地）
adb shell rm data/abcde1.jpg

------------------------------
对设备进行录屏
------------------------------
screenrecord /data/1.mp4

抓捕投焦数据
logcat | grep svt

修改设备尺寸密度（分倍率）
wm size
wm size 1280x720
wm size 1920x1080
(查看当前密度比)
wm density 
wm density 320 （修改当前密度比为320）
values-sw720dp：240          
values-sw540dp：320

进入到电脑桌面
C:\Users\xiaoyh>cd Desktop
将属性文件导入电脑桌面
C:\Users\xiaoyh\Desktop>adb pull vendor/build.prop
将内部配置 文件到入桌面
C:\Users\xiaoyh\Desktop>adb pull /system/etc/general_config.xml
如：<config name="CONFIG_LAYOUT_FOCUS_SETTINGS" value="true"/> //取消自动对焦

查找文件路径
busybox find . -name ir_config.ini
./vendor/tvconfig/config/ir_config.ini

getprop命令的作用就是从系统的各种配置文件中读取一些设备的信息。
 有 init.rc   default.prop    /system/build.prop
setprop可以对手机一些配置进行设置，当然这些配置必须是可写的。
命令格式：setprop [key] [value]
如：
setprop persist.homepage.mode 58   //修改ui界面
getprop persist.homepage.mode
> 58

查看ui界面所有进程
ps -A | grep ui
system        3744  2760  916028  78660 SyS_epoll_wait      0 S mediatek.factorymenu.ui
system        5939  2760  920648 117856 SyS_epoll_wait      0 S com.siviton.systemui
杀死进程
kill 5939
kill -s 9 进程号

查看内存分配空间(重要)
df -h
u盘目录
/mnt/runtime/default/18CF-F973
/storage/18CF-F973

vi编辑
busybox vi FullHD_CMO216_H1L01.ini

超级用户模式
su

---------------------------
在设备中修改屏参文件步骤
在mboot中输入
setenv UARTOnOff on
setenv factory_poweron_mode direct;save;reset
通过串口进入设备中
修改完屏参文件后
输入reboot，按住Enter键回到mboot模式
在mboot中输入
set db_table 0;save;reset
再次重启开机
----------------------------

工厂 设备测试
am start com.siviton.factory.autotest/.MainActivity

获取当前的语言
getprop persist.sys.locale
en-US

设置屏保为15秒
settings put System screen_off_timeout 15000
150秒
settings put System screen_off_timeout 150000


临时修改 netrange 的MAC地址
busybox ifconfig eth0 hw ether 40:e7:93:25:85:93
-------------测试mac地址
40e793258590

40e793258591

40e793258592

40e793258593

40e793258594

40e793258595

40e793258596

40e793258597

40e793258598

40e793258599
--------------

进入新的OTA升级界面
am start com.joyar.ota/com.joyar.ota.activity.LaunchActivity

--------------------
threadtime 抓log
设置 崩溃日志、主日志、系统日志 的大小为10MB
setprop persist.logd.size.crash 10485760

setprop persist.logd.size.main 10485760

setprop persist.logd.size.system 10485760

抓捕log  threadtime 可以在log前面加上线程时间和线程ID
logcat -c;logcat -v threadtime > /data/test.log
---------------------

查看init.fusion.rc 中 write /proc/lmk_privilege_list/name "值" 的 值的获取  (一般为15字)
U5:/ # ps -A | grep newos
u0_a15        4301  2798 1399332 186056 SyS_epoll_wait      0 S com.hulian.newos
u0_a15        4480  2798 1169656  37560 SyS_epoll_wait      0 S com.hulian.newos:data
U5:/ # ls
acct       charger default.prop init                    init.usb.configfs.rc mnt  product sys
bin        config  dev          init.environ.rc         init.usb.rc          odm  sbin    system
bugreports d       etc          init.rc                 init.zygote32.rc     oem  sdcard  ueventd.rc
cache      data    factory      init.recovery.mt5862.rc lost+found           proc storage vendor
U5:/ # cat /proc/4301/co
comm             coredump_filter
U5:/ # cat /proc/4301/comm
om.hulian.newos
-----------------------------------------

获取所有包名
pm list packages
-----------------------

复制目录进U盘
df -h 查看U盘路径
cp -r ./com.disney.disneyplus-avbS6TvkDa2l8jXuh6rL1Q== /mnt/runtime/default/667A-9C6C/

查看窗口信息
dumpsys window
dumpsys window windows

关闭设置apk
am force-stop com.joyar.general.settings

启动设置界面
am start -n com.joyar.general.settings/.ui.activity.SettingsActivity

启动锁屏界面
am start -n com.siviton.controlmachine/com.siviton.controlmachine.MainActivity
am start -n com.joyar.controlmachine/com.joyar.controlmachine.MainActivity

启动JoyarOTA界面
am start -n com.joyar.ota/com.joyar.ota.activity.LaunchActivity

启动CJWelcomeGuide 开机引导 界面
注意：复位会导致push 进去的 apk 变成原来的apk
备注：要在开机引导的时候连接adb push进去，不然会执行禁止启动的代码，am start 会无法启动开机引导
am start -n com.siviton.welcomeGuide/com.siviton.welcomeGuide.MainActivity
-------------------------------------------------------------------------

查看设备中的所有属性
getprop

查看设备的mac地址
cat /sys/class/net/eth0/address
getprop persist.svt.mac

查看设备的无线mac地址 (网络mac地址)
cat /sys/class/net/wlan0/address

查看网口mac地址
ifconfig eth0

-----------------------------------------------------------------------
查看设备的蓝牙是否打开
settings get global bluetooth_on
1 --- 打开
0 --- 关闭

--------------------------------------
起调服务
am startservice -n <包名>/<服务类名> -a <Action>
am startservice -n <包名>/<服务类名> -a android.intent.action.START_LEBO_SERVICE
am startservice -n com.hpplay.happyplay.aw/com.hpplay.happyplay.aw.AirPlayService -a android.intent.action.START_LEBO_SERVICE

-------------------------------------------------------
手动发送广播
adb shell am broadcast -a android.intent.action.BOOT_COMPLETED -n 包名/.你的Receiver路径
adb shell am broadcast -a android.intent.action.BOOT_COMPLETED -n com.joyar.ota/.receiver.OtaReceiver


------------------------------------------------------
获取投影角度相关信息
logcat | grep joyar_so

-----------------------------------------------------
抓取所有报错log
adb logcat *:E
logcat | grep *:E
     
-------------------------------------
关闭旋转梯形
setprop persist.trapezoid.ro.correct 0
sync

--------------------------------------
指令获取摄像头画面 (梯形算法获取的画面)
setprop persist.joyar.action.mode 6
adb pull /mnt/sdcard/DCIM/CamPreview.jpg
```
