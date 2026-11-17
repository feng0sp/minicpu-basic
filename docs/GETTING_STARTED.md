# 环境与运行步骤

当前主工程位于 `final_cpu/`。以下 Windows 命令从仓库根目录执行，Tcl 脚本会依据脚本自身位置定位工程，不要求将仓库放在特定盘符。

## 1 获取代码

```powershell
git clone https://github.com/bighu666-six/minicpu-basic.git
Set-Location .\minicpu-basic
$repoRoot = (Get-Location).Path
```

在已配置 Vivado 的 PowerShell 环境中定位工具：

```powershell
$vivado = (Get-Command vivado.bat -ErrorAction Stop).Source
```

若提示找不到 `vivado.bat`，请先配置本机 Vivado 2019.2 的环境，或把 `$vivado` 设为实际安装位置下 `vivado.bat` 的完整路径。后续命令共用这两个变量。命令展示的是运行步骤，不代表已经在当前机器上验证通过。

## 2 创建主工程

```powershell
& $vivado -mode batch -source (Join-Path $repoRoot 'final_cpu/run_vivado/create_board_project.tcl')
```

成功创建后，Vivado GUI 工程位于：

```text
final_cpu/run_vivado/project_lcd/final_cpu_lcd.xpr
```

脚本设置目标器件为 `xc7a200tfbg676-1`、综合顶层为 `soc_lite_lcd_top`，并导入 CPU、SoC、显示模块、IP 配置、引脚约束和仿真平台。`final_cpu/lib/lcd_module_cell.dcp` 是现有工程依赖。

`project_lcd/` 与 IP 生成文件由 Vivado 建立，不需要预先从仓库下载生成后的 GUI 工程。若换用其他开发板，需要先核对器件、时钟、引脚和 LCD 依赖，不能直接套用现有约束。

## 3 运行仿真

外设单元仿真：

```powershell
& $vivado -mode batch -source (Join-Path $repoRoot 'final_cpu/run_vivado/run_lcd_unit_sim.tcl')
```

脚本依次运行像素渲染、LCD 顶层和游戏外设三个 testbench。也可只运行其中一个：

```powershell
$env:LCD_UNIT_TOP = 'tb_game_peripherals'
try {
    & $vivado -mode batch -source (Join-Path $repoRoot 'final_cpu/run_vivado/run_lcd_unit_sim.tcl')
} finally {
    Remove-Item Env:LCD_UNIT_TOP -ErrorAction SilentlyContinue
}
```

完整 SoC 冒烟仿真：

```powershell
& $vivado -mode batch -source (Join-Path $repoRoot 'final_cpu/run_vivado/run_lcd_sim.tcl')
```

该脚本运行 `tb_lcd_top`。各测试具体检查的内容和结果判读方法见 [验证说明](VERIFICATION.md)。

## 4 构建与运行硬件

```powershell
& $vivado -mode batch -source (Join-Path $repoRoot 'final_cpu/run_vivado/run_lcd_impl.tcl')
```

也可以打开生成的 XPR，在 GUI 中运行综合、实现和 Generate Bitstream。下载到开发板之前应核对实际板卡与 [`soc_lite_top.xdc`](../final_cpu/run_vivado/constraints/soc_lite_top.xdc)、[`lcd_touch.xdc`](../final_cpu/run_vivado/constraints/lcd_touch.xdc) 的对应关系。

现有板级顶层的 `GAME_LCD` 默认值为 `0`，LCD 使用调试显示分支；不要把默认工程直接描述成 LCD 赛车画面。游戏画面分支和 VGA 路径请结合 [主工程说明](../final_cpu/README.md) 及实际演示配置核对。

## 5 重新编译游戏软件

仅使用仓库已有 BRAM 镜像时，可以先阅读和运行现有硬件工程。修改裸机软件后，才需要在 Linux / WSL 中重新构建。

从 WSL 中实际克隆位置的仓库根目录执行：

```bash
cd final_cpu/sw/game
make
```

默认交叉编译工具前缀为 `/opt/loongarch32r/bin/loongarch32r-linux-gnusf-`。安装在其他位置时，通过 Makefile 的 `TOOLPREFIX` 参数指定实际前缀。

`make` 生成 ELF、BIN、反汇编与指令 RAM 镜像等构建输出；`make install` 进一步覆盖 `final_cpu/mem/exp23/` 中的 `inst_ram.mif` 和 `inst_ram.coe`。安装新镜像后需要按实际用途重新运行仿真或实现，不能沿用旧结果作为新镜像的验证记录。

## 基础 CPU 实验入口

| 环境 | 用途 | 当前说明 |
| --- | --- | --- |
| `cdp_ede_local-master/` | 单周期数据通路与 trace 对拍 | [目录说明](../cdp_ede_local-master/README.md) · [数据通路审查](../cdp_ede_local-master/mycpu_env/myCPU/SINGLE_CYCLE_DATAPATH_AUDIT.md) |
| `cdp_ede_pipeline/` | 流水线、冒险处理与 trace 对拍 | [目录说明](../cdp_ede_pipeline/README.md) |

当前子目录说明以 `soc_bram` 为主要 trace 和板级环境，`soc_dram` 保留为兼容参考。旧根目录记录中的 EXP6 与当前单周期说明中的 EXP16 有差异，执行前应根据课程任务与当前环境确认实验编号。

部分旧脚本与说明仍含原机器绝对路径，例如 `scripts/build_func_exp6.sh` 中的 `REPO_ROOT`。它们保留在历史环境中，本页主工程步骤不依赖该脚本。
