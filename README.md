# OPENCORE-MSI-B460-WIFI

## 升级日志
### 2023-06-14


### 配置
｜名称｜品牌｜型号｜

## 硬件测试配置

| 部件      | 型号                     |
| -------- | ------------------------ |
| CPU      | Intel Core i5-10400      |
| 主板     | 微星 B460M MORTAR WIFI    |
| 内存     |  DDR4  8GB × 2           |
| 核显     | Intel UHD630             |
| 独显     | 蓝宝石 RX580              |
| 有线网卡 | RTL8125（板载）            |
| 无线网卡 | Intel AX200（板载）        |
| 硬盘     |    |


## USB

端口列表

| 名称 | 类型             | 位置                      | 是否定制 |
| ---- | ---------------- | ------------------------- | --- |
| HS01 | USB2.0           | 后置面板网口旁USB-A口 1    | [x] |
| HS02 | USB2.0           | 后置面板网口旁USB-A口 2    | [x] |
| HS03 | USB2.0           | 后置面板USB-C口旁USB-A口   | [x] |
| HS04 | USB2.0 Type-C    | 后置面板USB-C口           | [x] |
| HS05 | USB2.0           | 主板扩展口JUSB3 1         |  |
| HS06 | USB2.0           | 主板扩展口JUSB3 2         |  |
| HS07 | USB2.0 Type-C    | 主板扩展口JUSB4           |  |
| HS08 | 内置蓝牙          | 主板内置                  | [x] |
| HS09 | USB2.0           | 后置面板PS2口旁USB-A口 1   | [x] |
| HS10 | USB2.0           | 后置面板PS2口旁USB-A口 2   | [x] |
| HS11 | 内置USB2.0Hub    | 主板扩展口JUSB1-2          |  |
| HS12 | 内置微星灯效控制   | 主板内置                   |  |
| SS01 | USB3.0           | 后置面板网口旁USB-A口 1    | [x] |
| SS02 | USB3.0           | 后置面板网口旁USB-A口 2    | [x] |
| SS03 | USB3.0           | 后置面板USB-C口旁USB-A口   | [x] |
| SS04 | USB3.0 Type-C    | 后置面板USB-C口           | [x] |
| SS05 | USB3.0           | 主板扩展口JUSB3 1         |  |
| SS06 | USB3.0           | 主板扩展口JUSB3 2         |  |
| SS07 | USB3.0 Type-C    | 主板扩展口JUSB4           |  |



### 问题
1. SSD硬盘启动时间漫长
原因：
三星的某些型号的SSD，比如我的970 EVO plus，执行TRIM的操作特别的慢。而在APFS上，如果启用了TRIM，macOS会在启动的时候执行一次TRIM操作释放未使用的空间，就会导致启动速度特别的慢。
解决：
可以通过升级OpenCore到 0.7.9 及以上版本，然后设置SetApfsTrimTimeout值为0（默认为-1）关闭启动时的TRIM操作以提升启动速度。

### OpenCore配置
Misc/Boot
HideAuxiliary 隐藏辅助项
|类型|默认|
|布尔|true|

PickerAttributes 开机引导菜单的属性

PickerVariant 启动管理器图标路径

Misc/Security

DmgLoading dmg加载
Disabled
Signed 仅加载 Apple 签名的 DMG 磁盘映像。由于 Apple 安全启动的设计，不管 Apple 安全启动是什么状态，Signed 策略都会允许加载任何 Apple 签名的 macOS Recovery，这可能并不总是令人满意。虽然使用已签名的 DMG 磁盘映像更可取，但验证磁盘映像签名可能会稍微减慢启动时间（最多1秒）
Any 任何 DMG 磁盘映像都会作为普通文件系统挂载。强烈不建议使用 Any 策略，当激活了 Apple 安全启动时会导致启动失败

NVRAM/Add
4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14
  UIScale 用户界面缩放比例的一字节数据。
    普通屏幕应为 01，HiDPI 屏幕应为 02
    
7C436110-AB2A-4BBB-A880-FE41995C9F82
  csr-active-config 系统完整性保护 (SIP) 的设置
    00000000 - SIP 完全启用 (0x0)
    
  prev-lang:kbd 定义默认键盘布局的 ASCII 字符串
  
  boot-args
  例：igfxfw=2 igfxonln=1 alcid=1 gfxrst=1 alcdelay=500 shikigva=80 -cdfon -igfxmpc
  igfxfw=2 可以提高核显的频率，改善性能
  igfxonln=1 使用核显输出时强制HDMI/DP始终在线，防止无法唤醒出现黑屏
  alcid=1 声卡注入ID
  gfxrst=1 宁愿在第二次启动阶段绘制Apple徽标，也不要复制帧缓冲区
  alcdelay=500 延迟500ms 完美解决声卡重启掉驱动
  shikigva=80 解决 DRM 的视频解码黑屏问题
  -cdfon 4K 60Hz 输出黑屏或崩溃
  -igfxmpc 4K 60Hz 输出黑屏或崩溃
  -v 啰嗦模式
  -s 单用户模式
  -x 安全模式
