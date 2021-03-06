# 禁止 S3 睡眠

## 描述

- 本方法解决某些机器 **睡眠失败** 问题，这是临时解决方案，非治本方法。 **禁止`S3`睡眠** 后机器将不再睡眠。
- **睡眠失败** 表现为：机器一旦睡眠就无法被唤醒，呈现死机状态，只能强制关机。
- **睡眠失败** 本质是机器一直停滞在睡眠过程，始终没有睡眠成功。
- 下列情况不属于本文所描述的 **睡眠失败**
  - 机器能够正常睡眠 **「呼吸灯闪烁」**，但刚刚睡眠成功就立即醒来
  - 机器能够正常睡眠 **「呼吸灯闪烁」**，但无法被唤醒
- 个别睡眠、唤醒正常的机器， **禁止 `S3` 睡眠** 也不会破坏睡眠系统。

## 禁止 S3 睡眠补丁

- ACPI 规范 定义了 `S3` 方法，通过二进制更名使其定义失效可以 **禁止`S3`睡眠**。有关 `S3` 方面内容参见《ACPI 规范》。

- 更名：`_S3` to `XS3`
  
  ```Text
  Find:     5F53335F
  Replace:  5853335F
  ```
  
- ***SSDT-S3-disable***

  ```Swift
  If (_OSI ("Darwin"))
  {
      /* Empty */
  }
  Else
  {
      Name (_S3, Package (0x04)
      {
          0x05,
          Zero,
          Zero,
          Zero
      })
  }
  ```
