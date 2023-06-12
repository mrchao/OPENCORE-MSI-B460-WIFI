# OPENCORE-MSI-B460-WIFI

### 问题
1. SSD硬盘启动时间漫长
原因：
三星的某些型号的SSD，比如我的970 EVO plus，执行TRIM的操作特别的慢。而在APFS上，如果启用了TRIM，macOS会在启动的时候执行一次TRIM操作释放未使用的空间，就会导致启动速度特别的慢。
解决：
可以通过升级OpenCore到 0.7.9 及以上版本，然后设置SetApfsTrimTimeout值为0（默认为-1）关闭启动时的TRIM操作以提升启动速度。
