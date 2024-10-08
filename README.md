# NSCSCC24 团队赛龙架构 U-Boot 2024.07-rc3

## 这是什么

U-Boot 是嵌入式行业的 Bootloader 标准，而官方仓库的 U-Boot 版本仍然为 2019，从 2019 到 2024 年，U-Boot 添加了很多新特性，或者修复了潜在的 bug。因此将大赛的 LoongArch 32 Reduced U-Boot 升级到 2024 年的版本是建设大赛生态、提升大赛开发体验的十分有必要的工作。

## 我们的工作

- 我们固定上游 U-Boot 为 2024.07-rc3 的提交，并且参考 RISC-V、MIPS 与大赛官方提供的2019 年 U-Boot 的代码，进行了移植工作

- 我们修复了大赛官方提供的 U-Boot 初始化异常、与主线代码差异过大影响正常功能使用、定时器不准、网卡效率低等问题

- 我们重构了网卡驱动，TFTP 下载内核速度由 PMON 的 70kb/s 提升到 400kb/s，实现秒进内核，补丁已提交开源社区

- 使用官方提供的 U-Boot 2019 或者 PMON2000 下载 9MiB 的内核镜像，需要 122 秒，而我们的 U-Boot 2024 仅需 25 秒

## 如何移植

- 下载 U-Boot 最新源代码，将其 `git reset` 到 2024.07-rc3 的提交

- 使用 `git am` 将我们的补丁合入你的代码

- 我们约定，我们的平台为：`la32r_chiplab`，意为基于 Chiplab 的 32 位龙架构平台，架构为：`LoongArch`

- 根据实际情况修改 `include/configs/la32r_chiplab.h` 与你的 `config`

- 我们的方案是使用 `RISC-V CLINT` 作为 33MHz 的计数器，我们在硬件上实现了 `Sifive CLINT`， 如果你的设计与我们不一样，需要修改这一部分的源代码：`drivers/timer/loongarch_32r_timer.c`，将其 `MMIO` 读取计数器值的代码改为使用内嵌汇编的方式执行 `rdcntxxxxx` 的指令读取计数器值。并且更改设备树文件
