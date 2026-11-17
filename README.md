# 小学期 CPU 设计与应用系统

计算机组成原理课程实践：从单周期数据通路、五级流水线到 LA32R CPU 演示系统，保留 RTL、可编辑图纸、控制信号表及配套软件工具。

这是**三人合作项目的个人整理仓库**，源自 [FFspiky/minicpu-basic](https://github.com/FFspiky/minicpu-basic)。杨博清（feng0sp）主要参与**数据通路图设计**。CPU、外设与工具链为团队工程成果，具体来源和整理范围见[项目说明](docs/TEAM_PROJECT.md)。

## 从哪里开始

| 想了解什么 | 入口 |
| --- | --- |
| 数据通路与部件连接 | [图纸阅读指南](draw.io/README.md)：5 份 drawio 文件、36 个页面及两份控制信号表 |
| 单周期与流水线的实现 | [单周期环境](cdp_ede_local-master/)、[流水线环境](cdp_ede_pipeline/) |
| CPU、UART、NAND 与程序装载 | [final_cpu 系统说明](final_cpu/README.md) |
| 汇编、镜像生成与网页工具 | [la32asm / LA32 Studio](final_cpu/tools/la32asm/README.md) |
| 本次实际检查结果 | [验证记录](docs/VALIDATION.md) |

建议先看 EXP6 的总图，沿“取指 → 译码 → 执行 → 访存 → 写回”理解数据流，再看 EXP16 的分模块图和对应 RTL。图纸索引保留原始页面名称，便于在 diagrams.net 中查找。

## 项目内容

- 单周期 CPU 与五级流水线 CPU 教学工程，以及数据通路、内部部件和控制信号资料。
- `final_cpu`：流水线 LA32R CPU、统一 RAM、Boot Monitor，以及 UART、NAND、VGA 等外设与演示程序。
- `la32asm` 与 LA32 Studio：汇编编码、镜像打包、网页编辑构建、串口下载和 NAND 程序槽管理。

上述为仓库包含的团队实现范围；个人贡献以项目说明中的记录为准。

## 启动软件工具

Windows 安装 Git 和 Python 3.10+ 后，在 PowerShell 执行：

```powershell
git -c core.longpaths=true clone https://github.com/feng0sp/minicpu-basic.git
cd .\minicpu-basic
.\LA32-Studio.cmd
```

首次运行可能需要联网安装 Python 依赖，浏览器默认访问 `http://127.0.0.1:8765`。独立汇编文件的构建不需要 WSL；C 程序及部分预设构建还需要 WSL 和 LA32R GCC，详见工具说明。

上板使用 Vivado 2019.2、适配开发板及串口连接。已有稳定 bitstream 和便携工具包可在[团队 stable-20260716 Release](https://github.com/FFspiky/minicpu-basic/releases/tag/stable-20260716) 获取；该 Release 早于这里同步的源码提交，不能视为当前源码的重新构建结果。大体积便携包不重复提交到本仓库。

## 版本与资料

本次同步团队 `tree` 分支的 `b03bdf82d5f9bf687993f56d3238041942b4828a`，保留原提交历史，在个人 fork 中补充导航、图纸索引和验证记录。团队原始工程说明保存在[原始 README](docs/TEAM_ORIGINAL_README.md)；其中固定盘符、旧分支和历史验收记录需结合当时环境理解。

本次本地软件测试通过 28 项，另有 1 项 GNU 赛车差分测试因缺少参考二进制而跳过；没有重新进行 Vivado 综合、时序分析或开发板验收。复现命令和检查范围见[验证记录](docs/VALIDATION.md)。
