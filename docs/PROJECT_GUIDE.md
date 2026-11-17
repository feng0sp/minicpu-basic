# 演示和项目讲解提纲

本项目可以围绕“一个裸机程序如何在 CPU 上运行，并驱动输入和显示”来讲解。先展示系统用途，再沿源码说明模块关系和验证方法。

## 建议阅读顺序

1. [`racing_game.c`](../final_cpu/sw/game/racing_game.c)：游戏如何更新状态。
2. [`racing_game_mmio.h`](../final_cpu/sw/game/racing_game_mmio.h)：软件如何访问外设寄存器。
3. [`confreg.v`](../final_cpu/rtl/soc/CONFREG/confreg.v)：硬件如何保存和暴露状态。
4. [`soc_lite_lcd_top.v`](../final_cpu/rtl/soc/soc_lite_lcd_top.v)：CPU SoC、输入、LCD、VGA 和排行榜如何组合。
5. [`la32_pipeline_core.v`](../final_cpu/rtl/cpu/la32_pipeline_core.v)：流水线如何推进、暂停和提交。
6. [验证说明](VERIFICATION.md)：哪些行为已有测试入口，以及如何取得实际验证结果。

## 一次简短演示

当前仓库尚未附演示照片或视频。准备真实演示时，可以按以下顺序组织约两分钟的内容：

- 介绍开发板、显示设备、输入设备和使用的代码版本。
- 展示程序启动、按键操作、暂停、恢复和复位。
- 展示 VGA 画面或 LCD 调试页；注明实际使用的 `GAME_LCD` 配置。
- 指出一个 MMIO 状态变化，再对应到相关的软件与硬件模块。
- 最后说明已经验证的内容，以及仍需改进的地方。

已有截图、视频或波形后，再把真实材料链接加入项目首页。示意结构图只能用于解释架构，不能代替运行或上板证据。

## 面试与答辩准备

| 话题 | 建议准备的解释 | 可查阅位置 |
| --- | --- | --- |
| 单周期与流水线 | 普通指令、BRAM 访存和乘除法的完成方式有什么差异 | [单周期审查](../cdp_ede_local-master/mycpu_env/myCPU/SINGLE_CYCLE_DATAPATH_AUDIT.md) · [`rtl/cpu/`](../final_cpu/rtl/cpu/) |
| 数据冒险 | 什么情况下使用前递，什么情况下需要暂停 | [`la32_forward_unit.v`](../final_cpu/rtl/cpu/la32_forward_unit.v) · [`la32_pipeline_control.v`](../final_cpu/rtl/cpu/la32_pipeline_control.v) |
| 软硬件接口 | 软件更新分数后，显示模块怎样得到一致的状态 | [`confreg.v`](../final_cpu/rtl/soc/CONFREG/confreg.v) · [`game_state_cdc.v`](../final_cpu/rtl/lcd/game_state_cdc.v) |
| 结果验证 | 哪个测试检查哪种行为，出现错误时如何定位 | [`run_vivado/sim/`](../final_cpu/run_vivado/sim/) |
| 工程复现 | 换一台机器需要哪些工具、依赖和配置 | [运行步骤](GETTING_STARTED.md) |

准备时应选一两处自己实际处理过的问题，解释“现象、原因、修改和复测”，并对应到代码或提交记录。尚未独立理解或验证的部分，可以说明正在学习，避免把整个工程的所有模块都当作个人已掌握的内容。

## 来源和个人贡献

CDP/EDE 目录、课程框架、第三方 IP / DCP 与个人修改应分别说明。当前仓库的文件布局可以帮助定位模块，但不能单凭仓库归属判断每个模块的原始作者。

后续结合课程材料补充：框架的来源链接或任务名称、个人负责的功能、可查阅的修改提交，以及使用开发工具或 AI 辅助完成的范围。描述应以实际工作为准；本提纲不替代个人分工记录，也不为第三方文件追加授权。

## 后续整理清单

- [ ] 补充实际板卡型号、接线说明和使用的显示配置。
- [ ] 录制一次可复现操作的短视频，并标注代码版本。
- [ ] 保留四项测试的一轮真实日志与必要波形。
- [ ] 补充与具体构建版本对应的时序和资源报告。
- [ ] 按原始课程材料与提交记录完善来源和个人贡献说明。
