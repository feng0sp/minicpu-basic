# 仿真范围与结果记录

仓库提供了下列测试平台与运行脚本。本页描述源码中的检查内容和复现入口，不把测试平台的存在当作当前版本已经通过测试的证明。

## 测试入口

| 测试平台 | 源码中覆盖的内容 | 执行入口 |
| --- | --- | --- |
| [`tb_racing_pixel_renderer`](../final_cpu/run_vivado/sim/tb_racing_pixel_renderer.v) | 赛车对象和旋转文字的像素检查 | `run_lcd_unit_sim.tcl` |
| [`tb_lcd_game_top`](../final_cpu/run_vivado/sim/tb_lcd_game_top.v) | LCD 时序、帧状态快照、像素流与顺时针映射 | `run_lcd_unit_sim.tcl` |
| [`tb_game_peripherals`](../final_cpu/run_vivado/sim/tb_game_peripherals.v) | PS/2 按键、排行榜排序、VGA 同步时序、坐标映射及分数显示相关检查 | `run_lcd_unit_sim.tcl` |
| [`tb_lcd_top`](../final_cpu/run_vivado/sim/tb_lcd_top.v) | CPU 启动赛车程序、MMIO 状态、开始/暂停/恢复/复位与 LCD 调试页冒烟检查 | `run_lcd_sim.tcl` |

运行命令见 [环境与运行步骤](GETTING_STARTED.md)。两个脚本都会先加载主工程创建脚本，因此仍依赖 Vivado 和现有 IP 工程环境。

## 如何判断结果

测试平台通过 `$display` 输出检查信息，失败分支包含 `FAIL:` 和 `$fatal`。各测试到达正常结束分支时输出的标记如下：

```text
PASS: renderer objects and rotated messages checked
PASS: LCD timing, frame snapshot, stream, and CW mapping checked
PASS: PS/2 keys, leaderboard, VGA timing, and CCW mapping checked
PASS: racing game CPU MMIO boot and LCD debug-page smoke test completed
```

这些是从源码提取的预期标记，不是本次运行日志。复现时需要检查相应测试完整结束，保留真实日志，关注编译失败、`FAIL:`、`$fatal`、超时和工具警告；不能只凭一个历史 PASS 字符串下结论。

## 目前可核对的范围

- 源码、四个测试平台和对应 Tcl 入口已纳入版本库。
- 本次展示整理检查了文档链接、模块与脚本路径，未在当前环境执行 Vivado 仿真、综合、实现或硬件下载。
- 仓库目前未提交本次源码版本对应的仿真日志、完整时序报告、板卡照片或演示视频。
- 原主工程说明记载了 `WNS = +0.247 ns`、`TNS = 0`；这是原文记录。缺少对应报告、构建版本和约束说明时，不将其当作当前提交的可复现测量结果。

## 后续结果记录

每轮真实验证应同时记录以下信息：

1. Git 提交 SHA、Vivado 版本、器件与使用的约束。
2. 使用的存储镜像、测试顶层、运行命令及环境变量。
3. 实际 PASS / FAIL、失败原因、修复提交和复测结果。
4. 代表性波形或运行截图；硬件验证另附板卡型号与演示配置。
5. 如记录时序或资源占用，应保留相应报告，注明结果对应的具体版本。

较大的工具生成目录继续保留在本地；公开材料可选择有代表性的结果摘要，避免将整个 Vivado 输出目录加入源码仓库。
