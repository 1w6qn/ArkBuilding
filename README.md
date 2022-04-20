# ArkBuilding 明日方舟基建排班计算器
## 简介
（项目内部缩写 ALBC: Albc-Like Building Calculator）
  
支持基于 Buff 和干员的建模进行单房间中一定时间内产能的模拟，并枚举出多个房间中模型的所有组合。
将组合加入到整数规划模型后可求出多房间产能总和最大的方案。

对于不便建模的情况（如多房间联动等），同样可创建特化组合模板将组合加入到整数规划模型中参与求解。

项目目前有支持 CMake 构建的 C++ 源码、命令行程序和可通过 C/C++ API 调用的静态库、动态库供集成。

## 特点
* 高性能：使用 Cbc 求解器
* 高产能：大多数情况比贪心算法更优

## 计算流程图
![](img/arkbuilding.png)

## 模型
// TODO: 模型

## 目前局限
* 目前的问题模型里只考虑最大化一定时间内产能，不考虑其他因素。
* 目前只进行了初步的建模 + 求解的实现。
* 欢迎提出 Issue 或 Pull Request 来改进。

## 二进制文件
见 [GitHub Release](https://github.com/closure-studio/ArkBuilding/releases)

| 系统  | 命令行程序 | 动态库 | 静态库 | 备注  |
|-----|-------|-----|-----|-----|
// TODO: Binaries

| 头文件名    | 描述     |
|---------|--------|
| albc.h  | C++ 接口 |
| calbc.h | C 接口   |

## 命令行程序使用方法
```console
$ albccli -h
Usage:
  albccli [options]
Available options:
  -p, --playerdata       Path to player data file. 
                           PATH                            : string
  -g, --gamedata         Path to Arknights building data file.
                           PATH                            : string
  -l, --log-level        Log level.
                           Default is WARN.
                           <DEBUG|INFO|WARN|ERROR>         : string
  -t, --model-max-time   Model time limit in seconds.
                           Default is 57600 (16 hours).
                           TIME                            : double
  -T, --sovle-max-time   Problem solving timeout in seconds.
                           Default is 20.
                           TIME                            : double
  -m, --test-mode        Test mode. Leave empty for normal mode.
                           <ONCE|SEQUENTIAL|PARALLEL>      : string
  -P, --test-param       Test param.
                           NUM_CONCURRENCY|NUM_ITERATIONS  : int
  -L, --lp-file          Generate a lp-format file describing the problem.
                              : FLAG
  -S, --solution-detail  Generate a text file describing all feasible solution.
                              : FLAG
  -a, --all-ops          Show all operators info.
                              : FLAG
  -h, --help             Produce help message.
                              : FLAG
```

除 ONCE 外， `-m {测试模式}` 应与 `-P {测试参数}` 同时使用。

| 测试模式       | 测试参数      |
|------------|-----------|
| ONCE       | 不要求测试参数   |
| SEQUENTIAL | 依次运行的测试次数 |
| PARALLEL   | 并行运行的测试次数 |

例（测试数据位于`test/`目录中）：
```console
$ albccli -p test/player_data.json -g test/building_data.json -m ONCE -l INFO -L -S -P
```
该命令将运行一次测试，并将包含描述整数规划问题的 .lp 格式文件、全部组合的详细信息输出到工作目录。

// TODO: More info

## API 使用
[API 示例](https://github.com/closure-studio/ArkBuilding/tree/main/src/examples/src)

## 构建源码（目前在 CMake + Windows MSYS2 MinGW / Linux GCC, x64 / ARM64 上经过测试） 
1. 环境
   * GCC 9.0+
   * CMake （未确定版本，如出现不兼容请尽量升级到最新版）

2. CMake
    ```console
    ~/ArkBuilding# mkdir build
    ~/ArkBuilding# cd build/
    ~/ArkBuilding/build# cmake ..
    ```
   
3. 编译 CMake 中的某个 Target
   * "albccli" : 命令行程序
   * "albc" : 动态库
   * "albc_static" : 静态库
   * "albcexample" : API示例，见[此处](#API-使用)

## 项目中使用的第三方库及资源
* [Kengxxiao/ArknightsGameData](https://github.com/Kengxxiao/ArknightsGameData) （干员数据、基建数据）
* [ampl/coin](https://github.com/ampl/coin) （Cbc 的 CMake 构建支持）
* [open-source-parsers/jsoncpp](https://github.com/open-source-parsers/jsoncpp)
* [cameron314/concurrentqueue](https://github.com/cameron314/concurrentqueue)
* [Fytch/ProgramOptions.hxx](https://github.com/Fytch/ProgramOptions.hxx)

## License
// TODO: license