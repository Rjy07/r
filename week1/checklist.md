# 第一周 · 基本工具安装 · 操作清单

> 配套正文：[基本工具安装.md](基本工具安装.md)
> 用法：按编号从上到下操作，每步先做「操作」，再对照「结果」。结果不符时看「避免」，不要跳步。
> 图号对应正文截图，点击可查看。

## A. 安装 VS Code

| # | 操作 | 结果 | 避免 |
|---|---|---|---|
| 1 | 核对材料：VS Code 安装包、MinGW-w64 压缩包（`.7z`）、解压工具 | 三样齐全；确定一个纯英文、无空格的安装/解压路径 | 去下载 MSYS2、`mingw-get` 或 Dev-Cpp 自带编译器 |
| 2 | 双击 VS Code 安装包 → UAC 点「是」→ 同意协议 → 安装位置和开始菜单保持默认 → 附加任务 **5 项全勾** → 安装 → 去掉「运行 Visual Studio Code」的勾 → 完成 | 安装完成（[图 10](images/10-vscode-finish.png)），VS Code 未启动 | 漏勾「添加到 PATH」；安装路径含中文或空格；现在先启动 VS Code |

## B. 安装 GCC（MinGW-w64）

| # | 操作 | 结果 | 避免 |
|---|---|---|---|
| 3 | 打开 <https://github.com/niXman/mingw-builds-binaries/releases> → 选最新版本 → 在 Assets 中下载 `x86_64-16.2.0-release-posix-seh-ucrt-rt_v14-rev1.7z`（约 103 MB） | 下载得到一个 `.7z` 文件（[图 11](images/11-mingw-releases-assets.png)） | 下成 `i686`、`win32`、`mcf`、`msvcrt` 版本；换成 MSYS2 路线 |
| 4 | 右键 `.7z` 文件 → 解压到 `C:\`（或另一个纯英文、无空格目录） | 得到 `C:\mingw64\bin`，其中有 `gcc.exe`、`g++.exe`、`gdb.exe`（[图 12](images/12-mingw-bin-contents.png)） | 解压到含中文或空格的路径；解压成 `C:\mingw64\mingw64\bin`；找 setup 安装程序 |
| 5 | 打开 `...\mingw64\bin` → 点击地址栏让路径变蓝 → `Ctrl + C` | 剪贴板里是**以 `\bin` 结尾**的完整路径 | 复制成外层 `mingw64` 目录；手工敲路径导致拼写错误 |

## C. 配置 PATH 并验证

| # | 操作 | 结果 | 避免 |
|---|---|---|---|
| 6 | `Win + S` 搜「编辑系统环境变量」→ 环境变量 → 用户变量中的 **Path** → 编辑 → 新建 → 粘贴 bin 路径 → 逐层点「确定」 | Path 列表新增一行，内容是你的 `...\mingw64\bin`（[图 16](images/16-env-new-path.png)、[图 17](images/17-env-confirm.png)） | 删除或覆盖原有 Path 条目；粘贴的路径漏掉结尾 `\bin`；只点一层「确定」就关窗 |
| 7 | 关闭所有命令行窗口和 VS Code | 没有安装前就开着的旧窗口 | 在旧窗口里验证，环境变量不会生效 |
| 8 | `Win + R` → `cmd` → 依次执行 `gcc --version`、`g++ --version`、`gdb --version` | 三条命令都打印版本信息（[图 19](images/19-verify-gcc.png)；图里是参考文章的 MSYS2 环境，版本号和字样会不同） | 只验证 `gcc`；出现 `'gcc' 不是内部或外部命令` 还继续往下做 |

## D. 配置 VS Code 扩展

| # | 操作 | 结果 | 避免 |
|---|---|---|---|
| 9 | （选做）`Ctrl + Shift + X` → 搜索 `Chinese` → 安装中文语言包 → 点「Change Language and Restart」 | 界面变成中文（[图 23](images/23-chinese-ui.png)） | 装完不重启就以为没生效 |
| 10 | 扩展面板搜索 `C++` → 选择 Microsoft 的 **C/C++** → 安装 | 扩展显示已安装（[图 24](images/24-ccpp-extension.png)） | 装成 C/C++ Extension Pack（非必需）；装第三方同名扩展 |

## E. 跑通 Hello World

| # | 操作 | 结果 | 避免 |
|---|---|---|---|
| 11 | 新建代码文件夹（如 `D:\Code\hello`）→ VS Code 点「文件 → 打开文件夹」→ 选择它 → 弹窗点「是，我信任此作者」 | 左侧资源管理器显示该文件夹（[图 29](images/29-explorer-icons.png)） | 路径含中文；只打开单个文件而不是文件夹 |
| 12 | 在文件夹里新建 `test` 子文件夹 → 新建 `test.c` → 输入 Hello World 代码 → `Ctrl + S` | 标签上的小圆点消失，文件按 C 语言高亮 | 文件名写成 `test.txt` 或没有后缀；不保存；使用中文标点或全角分号 |
| 13 | 点击右上角 ▶ → 在下拉列表选带 `C:\mingw64\bin\gcc.exe` 的一项 → 弹窗点「是」 | 终端打印 `Hello, World!`，左侧出现 `.vscode/tasks.json` | 选错编译器；代码没保存；把正文图 36 的 `Hello Grayson~...` 当成自己的输出 |

## F. 断点调试

| # | 操作 | 结果 | 避免 |
|---|---|---|---|
| 14 | 把代码换成带循环的版本 → 在第 7 行 `printf` 左侧空白处单击 → 按 `F5` | 程序停在断点行，左侧出现调试工具条（[图 37](images/37-breakpoint.png)、[图 38](images/38-debug-panel.png)；图 38 是参考文章代码，`printf` 在第 6 行，以你自己的行号为准） | 点在行号文字上；代码未保存导致断点位置不对 |
| 15 | 鼠标悬停 `i` 查看当前值 → 连续点击「逐过程」5 次 | `i` 依次为 `0、1、2、3、4`，随后程序结束 | 把「逐过程」和「单步调试」混用；不打断点直接等结果 |

## G. 选做：多文件编译与调试

| # | 操作 | 结果 | 避免 |
|---|---|---|---|
| 16 | 打开 `.vscode/tasks.json`，把 `"${file}"` 改为 `"*.c"`，把输出名改为 `${fileDirname}\\program.exe` → `Ctrl + S` | 保存成功，无红色报错（[图 41](images/41-tasks-json-edit.png)） | 改错引号或逗号；修改后不保存 |
| 17 | 运行与调试 → 创建 `launch.json` → 选 **C++ (GDB/LLDB)** → 添加配置 **C/C++: (gdb) 启动** → 修改 `program` 和 `miDebuggerPath` → `Ctrl + S` | `launch.json` 无语法错误（[图 45](images/45-launch-json-modify.png)、[图 46](images/46-backslash-escape.png)） | 路径写单反斜杠 `\`；`program` 与 tasks 的输出名不一致；仍填参考文章的 `msys64` 路径 |
| 18 | 新建 `test2`，放入 `test.c`、`max.c`、`max.h` → `F5` → 分别试「逐过程」「单步调试」「单步跳出」 | 终端输出 `20`；「单步调试」能进入 `findMaxNum`（[图 47](images/47-multi-file-layout.png) 至 [图 50](images/50-step-out.png)） | `max.c` 没保存；`#include "max.h"` 写成尖括号；头文件名与代码不一致 |
