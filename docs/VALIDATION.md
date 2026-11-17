# 本次整理验证记录

检查日期：2026-11-17。代码基线为团队 `tree` 分支提交 `b03bdf82d5f9bf687993f56d3238041942b4828a`，文档在此基础上整理。

## 软件测试

使用 Windows 本地已有 Python 虚拟环境，运行团队仓库中的 unittest 测试。发现 29 项测试，**28 项通过、1 项跳过**，无失败。

通过项覆盖汇编编码、标签和立即数错误处理、镜像布局与 CRC、串口帧与下载流程，以及 Studio 汇编构建和下载接口。串口测试使用模拟对象，不代表实际串口或开发板已验收。

跳过项为 `test_gcc_assembly_matches_existing_gnu_binary`：本地没有生成 `final_cpu/sw/game/obj/racing_game.bin`，无法进行 GNU 赛车二进制差分。本次没有补造参考文件。

从完整克隆的仓库根目录，可新建环境复现软件测试：

```powershell
cd .\final_cpu\tools\la32asm
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
$env:PYTHONPATH=(Get-Location).Path
.\.venv\Scripts\python.exe -m unittest discover -s tests -v
```

持续集成在 Ubuntu 上运行相同测试；GNU 差分项需要 WSL 和已生成的参考文件，不由该工作流覆盖。CI 结果以仓库 Actions 实际记录为准。

## 图纸与文档

- 5 个 drawio 文件均可作为 XML 解析，共 36 页；检查了页面中的单元 ID 和连线引用。此项只检查文件结构，不等同于信号或电路正确性验证。
- 新增文档中的本地路径对照 Git 跟踪文件清单检查，包括中文、括号及空格文件名。
- 原始 drawio、控制信号表、RTL 和软件实现保持同步基线内容；本次新增修改集中在导航、文档和测试工作流。

## 本次未执行

未重新运行 Vivado 仿真、综合、布局布线、时序分析或开发板验证。团队历史上板与时序记录仍保留在[原始说明](TEAM_ORIGINAL_README.md)和 [final_cpu 说明](../final_cpu/README.md)中，不视为本次复验结果。

团队 [stable-20260716 Release](https://github.com/FFspiky/minicpu-basic/releases/tag/stable-20260716) 的便携包及 bitstream 是独立的历史发布物，未在本次重新打包或重新生成。
