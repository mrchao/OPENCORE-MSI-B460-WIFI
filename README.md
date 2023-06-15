# OPENCORE-MSI-B460-WIFI

## 升级日志
### 2023-06-14
OC版本：0.88

## 硬件配置

| 部件      | 型号                     |
| -------- | ------------------------ |
| CPU      | Intel Core i5-10400      |
| 主板     | 微星 B460M MORTAR WIFI    |
| 内存     | DDR4 8GB × 2           |
| 核显     | Intel UHD630             |
| 独显     | 蓝宝石 RX580              |
| 声卡     | Realtek ALC1200 |
| 有线网卡 | RTL8125（板载）            |
| 无线网卡/蓝牙 | Intel AX200（板载）    |
| 无线网卡/蓝牙 | BCM943602CS（PCIE）   |
| 硬盘     | 三星 970 EVO 256G（MacOS）|
| 硬盘     | 三星 860 EVO 256G（Win10）|
|显示器|Dell 4K（DP）|


## 主板设置
| 名称 | 参数 |
| -------- | ------------------------ |
|BIOS 版本|7C82v14|
|D.T.M|允许|

## 功能测试
- [x] 核显加速
- [x] 五项节能
  - 需要关闭主板`xx`
  - 需要添加`SSDT-PM.aml`
- [x] 板栽声卡
- [x] 板载网卡
- [ ] 板载无线
- [ ] 板载蓝牙
- [ ] USB
- [ ] 睡眠/唤醒
- [x] 隔空投送（airdrop）
- [x] 通用控制（Universal Control）
- [x] 接力（hardoff） 
- [x] 随航（sidecar）
- [x] watch解锁


## USB

端口列表

| 名称 | 类型             | 位置                      | 是否定制 |
| ---- | ---------------- | ------------------------- | --- |
| HS01 | USB2.0           | 后置面板网口旁USB-A口 1    | - [x] |
| HS02 | USB2.0           | 后置面板网口旁USB-A口 2    | [x] |
| HS03 | USB2.0           | 后置面板USB-C口旁USB-A口   | [x] |
| HS04 | USB2.0 Type-C    | 后置面板USB-C口           |  |
| HS05 | USB2.0 Type-C    | 主板扩展口JUSB3 1         | [x] |
| HS06 | USB2.0           | 主板扩展口JUSB3 2         | 可能接前面板 USB3 [x] |
| HS07 | USB2.0           | 主板扩展口JUSB4           | 可能接前面板 Type-C |
| HS08 | 内置蓝牙          | 主板内置                  | [x] |
| HS09 | USB2.0           | 后置面板PS2口旁USB-A口 1   | [x] |
| HS10 | USB2.0           | 后置面板PS2口旁USB-A口 2   | [x] |
| HS11 | 内置USB2.0Hub    | 主板扩展口JUSB1-2          | [x] |
| HS12 | 内置微星灯效控制   | 主板内置                   |  |
| SS01 | USB3.0           | 后置面板网口旁USB-A口 1    | [x] |
| SS02 | USB3.0           | 后置面板网口旁USB-A口 2    | [x] |
| SS03 | USB3.0           | 后置面板USB-C口旁USB-A口   | [x] |
| SS04 | USB3.0 Type-C    | 后置面板USB-C口           | [x] |
| SS05 | USB3.0 Type-C    | 主板扩展口JUSB3 1         | [x] |
| SS06 | USB3.0           | 主板扩展口JUSB3 2         | 可能接前面板 USB3 [x] |
| SS07 | USB3.0           | 主板扩展口JUSB4           | 可能接前面板 Type-C |

<img width="360" alt="MSI-B460-WIFI" src="https://github.com/mrchao/OPENCORE-MSI-B460-WIFI/assets/5938440/0709ed8d-c2ec-4598-9096-7430d55b4724" />

特殊端口说明：

- HS08：蓝牙端口，必须包含，否则蓝牙失效
- HS11：内部USB2.0Hub端口，如果未使用JUSB1-2扩展口则可以屏蔽
- HS12：微星灯效端口，mac下没有适配，可屏蔽
---

windows端口检测结果：

<img width="720" alt="USB定制" src="https://github.com/mrchao/OPENCORE-MSI-B460-WIFI/assets/5938440/5870dea3-d489-4352-8aa8-195fbed994df">


---
定制流程参考 -> [黑苹果屋](http://imacos.top/2022/08/22/windows-usb-macos-bigsur-11-3-usbtoolbox/)
---

### 故障问题及处理
- SSD硬盘启动时间漫长 [x]

  `原因：`
  > 三星的某些型号的SSD，比如我的970 EVO plus，执行TRIM的操作特别的慢。而在APFS上，如果启用了TRIM，macOS会在启动的时候执行一次TRIM操作释放未使用的空间，就会导致启动速度特别的慢。

  `解决：`
  > 可以通过升级OpenCore到 0.7.9 及以上版本，然后设置SetApfsTrimTimeout值为0（默认为-1）关闭启动时的TRIM操作以提升启动速度。

- 隔空投送只能接收无法发送 [x]

  `原因：`
  > 查看 系统偏好设置 -> 安全性与隐私 -> 隐私 -> 蓝牙 允许下面的APP使用蓝牙

  `解决：`
  > 添加 AirPort实用工具

### OpenCore配置 

ACPI

| name | desc |
| ---- | ---- |
| SSDT-PM.aml | 节能第5项（断电后重启）| 

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
