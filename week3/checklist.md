# 第三周 · CMake 安装与使用 · 操作清单

> 配套正文：[CMake安装与使用.md](CMake安装与使用.md)
> 用法：按编号从上到下操作，每步先做「操作」，再对照「结果」。结果不符时看「避免」，不要跳步。
> 图号对应正文截图，点击可查看。

## A. 安装并验证 CMake

| # | 操作 | 结果 | 避免 |
|---|---|---|---|
| 1 | 新开一个终端，执行 `gcc --version`、`mingw32-make --version` | 两条命令都输出版本信息 | 使用第一周遗留的旧终端；`mingw32-make` 找不到还继续 |
| 2 | 打开 <https://cmake.org/download/> → 在 **Latest Release** 中下载 `cmake-4.4.4-windows-x86_64.msi`（约 37.3 MB） | 得到一个 `.msi` 安装包 | 下载 `4.5.0-rc1` 候选版；下载 `i386`、`arm64` 或 `.zip` 版 |
| 3 | 双击 `.msi` → UAC 点「是」→ 接受协议 → **勾选 Add CMake to the PATH environment variable** → 安装路径保持 `C:\Program Files\CMake` → Install → Finish | 安装完成（[图 1](images/01-cmake-install-options.png)；图里版本显示为 4.5.0-rc1，你装 4.4.4 时版本号不同） | 漏勾 PATH 选项；安装路径含中文或空格 |
| 4 | 关闭所有终端和 VS Code → 重新打开 → 依次执行 `cmake --version`、`where.exe cmake`、`mingw32-make --version` | cmake 版本 ≥ 3.20（助教写作时为 4.4.4）；`where.exe` 指向 `C:\Program Files\CMake\bin\cmake.exe`；make 有版本输出 | 用安装前就开着的旧窗口；不核对 `where.exe` 指向的路径 |

## B. 第一个单文件项目

| # | 操作 | 结果 | 避免 |
|---|---|---|---|
| 5 | 在资源管理器新建 `D:\Code\cmake-demo\hello`（没有 D 盘就改用 C 盘的纯英文目录）→ VS Code 打开 `cmake-demo` 这一层 → 点「是，我信任此作者」 | 左侧资源管理器显示 `cmake-demo`，里面有 `hello` | 路径含中文或空格；以为 `D:\Code\cmake-demo` 是电脑自带目录；打开到错误的层级 |
| 6 | 在 `hello` 里新建 `hello.c` 和 `CMakeLists.txt`（大小写完全一致）→ 粘贴正文代码 → 两个文件都按 `Ctrl + S` | 两个标签上的小圆点都消失 | 写成 `CMakeList.txt`、`cmakelists.txt`；文件变成 `hello.c.txt`；写 `VERSION 2.8` |
| 7 | 终端切到 `...\cmake-demo\hello` → 执行 `cmake -S . -B build -G "MinGW Makefiles"` | 输出 `Configuring done`、`Generating done`、`Build files have been written to...`，左侧出现 `build` 文件夹 | `-G` 后面的引号省略；不在 `hello` 目录里执行；`CMakeLists.txt` 没保存 |
| 8 | 执行 `cmake --build build` | 输出 `Building C object ...`、`Linking C executable hello.exe`、`Built target hello` | 修改代码后没保存；跳过 `cmake --build` 直接敲 `make` |
| 9 | 运行程序：PowerShell 用 `.\build\hello.exe`；CMD 用 `build\hello.exe`；Git Bash 用 `./build/hello.exe` | 终端输出 `Hello, CMake!` | 用错终端的路径写法；在错误的当前目录执行 |
| 10 | 修改 `hello.c` → `Ctrl + S` → `cmake --build build` → 再次运行 | 输出变成修改后的内容 | 改了代码没保存；只改注释却期待输出变化 |
| 11 | 构建结果异常时：`Remove-Item -Recurse -Force build` → 重新执行配置命令 → 再构建 | `build` 重新生成，构建成功 | 删掉源码；删了 `build` 却没有重新配置 |

## C. 多文件项目与目录结构

| # | 操作 | 结果 | 避免 |
|---|---|---|---|
| 12 | 新建 `calculator` 文件夹，创建 `main.c`、`add.c`、`add.h`、`sub.c`、`sub.h`、`CMakeLists.txt` → 粘贴正文代码 → 全部保存 | 文件结构与正文第 5.1 节一致 | 漏建文件；头文件名与 `#include` 不一致；函数名拼写不一致 |
| 13 | `CMakeLists.txt` 写 `add_executable(calculator main.c add.c sub.c)` → 配置、构建、运行 | 输出 `10 + 3 = 13` 和 `10 - 3 = 7` | 漏写 `add.c` 或 `sub.c`，链接报 `undefined reference`；把 `.h` 也列进去；写 `VERSION 2.8` |
| 14 | 把 `add_executable` 改成 `aux_source_directory(. SRC_LIST)` + `add_executable(calculator ${SRC_LIST})` → 构建、运行 | 输出与上一步相同 | 同目录放了带 `main` 的练习文件，报 `multiple definition of main`；以为它会递归收集子目录 |
| 15 | 新建 `inc/`、`src/`，按正文第 7 节移动文件；`CMakeLists.txt` 加入 `include_directories(${CMAKE_CURRENT_SOURCE_DIR}/inc)` 和 `aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/src SRC_LIST)` → 重新配置、构建、运行 | 输出仍为 `10 + 3 = 13`、`10 - 3 = 7` | 忘记 `include_directories`，报 `fatal error: add.h: No such file or directory`；目录名写错；文件没有真的移动 |

## D. 编译选项

| # | 操作 | 结果 | 避免 |
|---|---|---|---|
| 16 | 在 `add_executable` **之前**加入 `add_compile_options(-Wall -Wextra -std=c11)` → 重新配置、构建 | 构建成功；有警告时会显示文件名和行号 | 写在 `add_executable` 后面；照抄 `-std=c++11` 或 `-O2` |
| 17 | 阅读构建输出里的 `warning:`，按文件、行号定位 | 能指出警告对应的文件和行号 | 把警告当成没看见，继续提交 |

## E. 选做：静态库

| # | 操作 | 结果 | 避免 |
|---|---|---|---|
| 18 | 按正文第 9 节建 `libdemo` → `CMakeLists.txt` 使用 `add_library(math_utils STATIC ...)` 和 `target_link_libraries(libdemo PRIVATE math_utils)` → 配置、构建、运行 | 输出 `square(7) = 49`；`build/` 中出现 `libmath_utils.a`（[图 7](images/07-static-library-output.png)） | 漏写 `target_link_libraries`，链接报 `undefined reference`；库源文件路径写错 |
