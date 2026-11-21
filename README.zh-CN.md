# SysY2026 编译器（LKC1128）— 中文说明

> 上游：`LKC1128/Compiler`，SysY2026 → ARMv8-A/AArch64，含 SSA IR、中端 Pass 与 AAPCS64 代码生成。本仓库为其镜像，仅新增 CI workflow 与中文说明。

## 版本对齐 SysY2026

- 仓库内自带 `docs/2026年全国大学生计算机系统能力大赛-编译系统设计赛-编译系统实现赛道技术方案.pdf`——即 2026 年华为毕昇杯官方技术方案（SysY2026 语言定义）。
- 同时提供 `docs/SysY2022-Language-Definition-V1-English.pdf` 与 Runtime 文档——对应 SysY2026 的 2022 基础。
- 目标平台：**ARMv8-A（AArch64）**，遵守 AAPCS64 调用约定；Makefile 默认编译器为 `clang++-18`（比赛指定基线）。

## 构建方法

纯 C++17，无外部库依赖。两种方式：

```bash
# 方式 1：比赛脚本（推荐，clang++-18）
sudo apt install -y clang-18 llvm-18-dev lld
make all                       # -> ./compiler

# 方式 2：CMake（含 ctest 回归）
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release -DBUILD_TESTING=ON
cmake --build build -j4
cd build && ctest --output-on-failure

# 冒烟测试
echo 'int main() { return 0; }' > /tmp/smoke.sy
./compiler /tmp/smoke.sy -S -o /tmp/smoke.s
```

## CI 状态

- GitHub Actions：`.github/workflows/build.yml`，安装 clang-18 → `make all` → ctest 回归 → 冒烟测试。
- 状态：✅ 通过（run `44903409351`，约 1m40s）。
- Gitee 镜像：`https://gitee.com/edidada/SysY2026-lkc1128`（`main`）。
- 本地：未编译，全部交给 CI。
