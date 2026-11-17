# 数据通路图纸阅读指南

本目录保留团队原始图纸和控制信号表。杨博清主要参与数据通路图设计；以下为团队资料索引，不逐份认定个人作者。

## 打开与阅读

下载 `.drawio` 文件后，使用 diagrams.net 的“打开现有图表”载入；多页文件通过底部页面标签切换。仓库不依赖在线预览服务，原文件可继续编辑。

建议先读 EXP6 单周期总图，确认 PC、译码、寄存器堆、ALU、访存和写回的连接；再对照五级流水线图找出段寄存器、冒险处理和控制信号；最后按 EXP16 分页查看取指、分支、LSU、CSR、异常和提交路径。

## 文件与页面

共 5 份 drawio 文件、36 个页面。下面保留原始页面名称与顺序，包括文件中的副本页。

### [单周期数据通路图(EXP6).drawio](%E5%8D%95%E5%91%A8%E6%9C%9F%E6%95%B0%E6%8D%AE%E9%80%9A%E8%B7%AF%E5%9B%BE%28EXP6%29.drawio)

1 个页面：

1. 第 1 页

### [五级流水线通路图(EXP6).drawio](%E4%BA%94%E7%BA%A7%E6%B5%81%E6%B0%B4%E7%BA%BF%E9%80%9A%E8%B7%AF%E5%9B%BE%28EXP6%29.drawio)

1 个页面：

1. 第 1 页

### [五级流水线部件结构(EXP6).drawio](%E4%BA%94%E7%BA%A7%E6%B5%81%E6%B0%B4%E7%BA%BF%E9%83%A8%E4%BB%B6%E7%BB%93%E6%9E%84%28EXP6%29.drawio)

1 个页面：

1. 第 1 页

### [单周期CPU及内部部件(EXP16).drawio](%E5%8D%95%E5%91%A8%E6%9C%9FCPU%E5%8F%8A%E5%86%85%E9%83%A8%E9%83%A8%E4%BB%B6%28EXP16%29.drawio)

14 个页面：

1. 00_EXP16非流水CPU顶层数据通路
2. 01_PC与同步取指预取控制
3. 01_PC与同步取指预取控制 的副本
4. 02_译码控制与立即数生成
5. 03_通用寄存器堆与地址选择
6. 04_ALU与源操作数选择
7. 05_MulDiv多周期单元与等待上下文
8. 06_分支判断与目标地址
9. 07_LSU与同步Load等待
10. 09_Store写掩码与写数据整理
11. 10_Load数据选择与扩展
12. 11_CSR访问适配与按位写入
13. 12_CSR寄存器组定时器与稳定计数器
14. 14_核心状态提交仲裁与最终写回

### [五级流水线CPU及内部部件(EXP16).drawio](%E4%BA%94%E7%BA%A7%E6%B5%81%E6%B0%B4%E7%BA%BFCPU%E5%8F%8A%E5%86%85%E9%83%A8%E9%83%A8%E4%BB%B6%28EXP16%29.drawio)

19 个页面：

1. LA32 EXP16 顶层数据通路（黑白补全版）
2. 01 Next PC 与 PC
3. 02 取指单元 I-Unit
4. 03 IF/ID 流水段寄存器
5. 04 控制译码器
6. 05 立即数生成器
7. 06 通用寄存器堆
8. 07 ID/EX 流水段寄存器
9. 08 执行模块 EXU
10. 09 跳转/分支判断模块
11. 10 EX/MEM 流水段寄存器
12. 11 LSU 访存处理
13. 12 Data RAM
14. 13 MEM/WB 流水段寄存器
15. 14 写回选择与提交门控
16. 15 冒险、前递与流水控制
17. 16 异常/中断控制器
18. 17 CSR 寄存器组
19. 18 64位稳定计数器

## 控制信号表

- [单周期CPU控制信号取值表(EXP6) .xlsx](%E5%8D%95%E5%91%A8%E6%9C%9FCPU%E6%8E%A7%E5%88%B6%E4%BF%A1%E5%8F%B7%E5%8F%96%E5%80%BC%E8%A1%A8%28EXP6%29%20.xlsx)
- [流水线控制信号取值表(EXP6) .xlsx](%E6%B5%81%E6%B0%B4%E7%BA%BF%E6%8E%A7%E5%88%B6%E4%BF%A1%E5%8F%B7%E5%8F%96%E5%80%BC%E8%A1%A8%28EXP6%29%20.xlsx)

## 与 RTL 对照

以下为 `final_cpu` 中的模块阅读入口，方便按功能查找；EXP6、EXP16 图纸和后续演示系统存在版本差异，不表示所有信号逐项一致。

| 图中关注点 | RTL 入口 |
| --- | --- |
| 流水线整体连接 | [la32_pipeline_core.v](../final_cpu/rtl/cpu/la32_pipeline_core.v) |
| PC 与取指 | [la32_pc.v](../final_cpu/rtl/cpu/la32_pc.v)、[la32_fetch_unit.v](../final_cpu/rtl/cpu/la32_fetch_unit.v) |
| 译码与立即数 | [la32_decoder.v](../final_cpu/rtl/cpu/la32_decoder.v)、[la32_imm_gen.v](../final_cpu/rtl/cpu/la32_imm_gen.v) |
| 寄存器堆与执行 | [regfile.v](../final_cpu/rtl/cpu/regfile.v)、[la32_exu.v](../final_cpu/rtl/cpu/la32_exu.v) |
| 访存、控制与写回 | [la32_lsu.v](../final_cpu/rtl/cpu/la32_lsu.v)、[la32_pipeline_control.v](../final_cpu/rtl/cpu/la32_pipeline_control.v)、[la32_wb_select.v](../final_cpu/rtl/cpu/la32_wb_select.v) |
| CSR、异常与计数器 | [la32_csr.v](../final_cpu/rtl/cpu/la32_csr.v)、[la32_exception_control.v](../final_cpu/rtl/cpu/la32_exception_control.v)、[la32_stable_counter.v](../final_cpu/rtl/cpu/la32_stable_counter.v) |

阅读时可以选一条普通运算指令和一条条件分支，沿图解释操作数从哪里来、经过哪些选择器、何时写回，以及流水线为什么需要暂停或前递。

图纸 XML 可读取不代表硬件功能已经验证。原始工程说明与本次检查记录分别见[团队资料](../docs/TEAM_ORIGINAL_README.md)和[验证记录](../docs/VALIDATION.md)。
